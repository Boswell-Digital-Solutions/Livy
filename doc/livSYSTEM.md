# Livy — Compiled System Reference

**Designation:** liv
**Document role:** Canonical compiled technical reference for the Livy history-tour app
**Source:** `doc/system/`
**Build command:** `bash doc/system/BUILD.sh`
**Document version:** 1.0 (2026-09-24) — initial doc/system authored from scratch
**Protocol:** BDS Documentation Protocol v2.0

> **Generated artifact warning:** `doc/livSYSTEM.md` is assembled output.
> Edit the source modules under `doc/system/` and rebuild. Hand edits to
> generated artifacts are overwritten by the next build.

This `doc/system/` tree is the canonical source of truth for Livy. It uses
explicit **truth classes**: canonical facts define role, architecture, and
what is actually implemented versus described-but-not-built; snapshot facts
are dated, audit-derived observations. This tree was authored directly from
the backend and frontend source, not from the (previously broken) README —
see `90-handover.md`.

Assembly contract:

- Command: `bash doc/system/BUILD.sh`
- Validation: `bash doc/system/validate_snapshots.sh` runs during assembly
- Primary output: `doc/livSYSTEM.md`

| Part | File | Contents |
| --- | --- | --- |
| §1 | `01-overview-philosophy.md` | Purpose, pilot tour, and the RAG-placeholder disclosure |
| §2 | `02-architecture.md` | FastAPI backend + SvelteKit frontend split |
| §3 | `03-tech-stack.md` | Stack details, including what's planned-not-implemented |
| §4 | `04-project-structure.md` | Directory layout |
| §5 | `05-config-env.md` | `SECRET_KEY` (required), `DATABASE_URL` (optional, defaults to SQLite) |
| §6 | `06-testing.md` | Test commands |
| §7 | `90-handover.md` | README/implementation gaps and what changed in this pass |

## Quick Assembly

```bash
bash doc/system/BUILD.sh
```

---

# Overview and Philosophy

Livy is a location-aware history-tour application: a map-based frontend
showing points of interest ("stops") along a tour, paired with a FastAPI
backend for auth, tour/stop data, and (currently placeholder) AI-assisted
Q&A about tour content.

**Canonical fact:** the pilot content is a "Lexington Heritage Loop" tour,
per this repo's own README.

**Canonical fact:** the backend uses JWT-based authentication (`python-jose`,
`passlib`) and requires a `SECRET_KEY` environment variable to be set — the
backend raises a `RuntimeError` at startup if it is not defined (see
`05-config-env.md`).

**Planned, not yet implemented:** the README describes an "AI-powered Q&A
(RAG-based)" feature using LangChain and a vector database (Weaviate,
Pinecone, or pgvector). The actual `backend/rag.py` module is an explicit
placeholder — its own code comment states "Placeholder for LangChain-based
retrieval augmented generation pipeline... In a real system this would use
embeddings stored in pgvector and an LLM," and its function returns a fixed
dummy string rather than performing retrieval or calling an LLM. Do not
describe RAG-based Q&A as a working feature based on the README alone.

---

# Architecture

Livy is a two-part application: a SvelteKit frontend and a FastAPI backend,
communicating over REST.

**Canonical fact — backend (`backend/`):** a flat module layout, not a
package-per-feature structure:

- `main.py` — FastAPI application entry point
- `auth.py` — JWT authentication (OAuth2 password flow, `python-jose`,
  `passlib`/bcrypt)
- `database.py` — SQLAlchemy engine/session setup (`create_engine`,
  `sessionmaker`, declarative `Base`)
- `models.py` — SQLAlchemy ORM models
- `schemas.py` — Pydantic request/response schemas
- `storage.py` — object storage via `boto3`, including presigned URL
  generation (`get_presigned_url`)
- `rag.py` — placeholder AI Q&A function; see `01-overview-philosophy.md`

**Canonical fact:** `pgvector.sqlalchemy.Vector` is imported in the backend,
implying a Postgres + pgvector column type is defined somewhere in
`models.py`, even though `rag.py` does not yet query it. The vector column
existing does not mean retrieval is implemented — see the placeholder note
above.

**Canonical fact — frontend (`frontend/`):** a SvelteKit application with
routes for `about`, `map`, and `tours`, using `maplibre-gl` for mapping.
This is the frontend's entire declared npm dependency beyond SvelteKit
itself — no LangChain, vector-DB client, or AI SDK is present on the
frontend side.

**Not yet established here:** whether the "Service Worker caching for
offline support" feature described in the README is implemented — no
service worker file was located directly under `frontend/src/` while
authoring this chapter. Verify directly in the frontend source before
relying on offline-support claims.

---

# Tech Stack

**Canonical fact — backend:**

- FastAPI
- SQLAlchemy (with a Postgres + `pgvector` column type imported, per
  `pgvector.sqlalchemy.Vector`)
- `python-jose` + `passlib` (JWT auth)
- `boto3` (object storage)

**Canonical fact — frontend:**

- SvelteKit
- `maplibre-gl` (mapping)
- Tailwind CSS, per the README (not independently re-verified against
  `package.json` while authoring this chapter)

**Planned, not yet implemented (per README, not confirmed in code beyond
an import):** LangChain, and a vector database chosen from Weaviate,
Pinecone, or pgvector for retrieval-augmented generation. See
`01-overview-philosophy.md`.

---

# Project Structure

```text
backend/
  main.py        FastAPI app entry point
  auth.py        JWT authentication
  database.py    SQLAlchemy engine/session setup
  models.py      ORM models
  schemas.py     Pydantic schemas
  storage.py     boto3 object storage, presigned URLs
  rag.py         Placeholder AI Q&A (not yet a real RAG pipeline)
frontend/
  src/lib/components/   UI components
  src/routes/           SvelteKit pages: about/, map/, tours/
  static/                Static assets
tests/           Test suite (see 06-testing.md)
```

---

# Configuration and Environment

**Canonical fact:** `SECRET_KEY` is required. `backend/auth.py` reads it with
`os.getenv("SECRET_KEY")` and raises `RuntimeError("SECRET_KEY environment
variable is not set")` at import time if it is absent — the backend cannot
start without it.

**Canonical fact:** `DATABASE_URL` is optional and defaults to a local
SQLite file: `backend/database.py` reads it with
`os.getenv("DATABASE_URL", "sqlite:///./app.db")`. Despite the README
describing Livy as using "a Postgres database," **the actual default,
unconfigured behavior is SQLite**. A Postgres connection string must be set
explicitly via `DATABASE_URL` to get Postgres (and pgvector) behavior.

**Not yet established here:** environment variables for `boto3` storage
credentials (`storage.py`) were not enumerated while authoring this
chapter — check `storage.py` directly, or the standard AWS SDK environment
variables (`AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY`, etc.), before
deploying.

---

# Testing

**Canonical fact:** the backend test suite is a single file,
`tests/test_app.py`. Run it with `pytest` from the repository root (the
`PYTHONPATH=. pytest` form is needed if `backend` is not otherwise
importable — see the repository README's troubleshooting section).

**Not yet established here:** no frontend test tooling (e.g. Vitest,
Playwright) was located while authoring this chapter. If frontend tests
exist, they were not found under `frontend/`; treat frontend changes as
manually verified until a test runner is confirmed.

---

# Handover

**Canonical fact:** the single biggest gap between this repo's README and
its actual implementation is the AI/RAG Q&A feature — see
`01-overview-philosophy.md`. Anyone picking up work on "AI Q&A" should
start from `backend/rag.py`'s current placeholder, not from the README's
feature description.

**Canonical fact:** the default local run uses SQLite (`DATABASE_URL`
unset), not Postgres — see `05-config-env.md`. A contributor following the
README's "Postgres database" framing without reading `database.py` first
would be surprised by this.

**Known limitation of this chapter set:** authored from a direct read of
`backend/*.py`'s imports and top-level logic, the frontend's route
structure and `package.json`, and the existing README — not from running
the application or its test suite. Roadmap items in the README (multi-region
expansion, premium/sponsor tiers) are not restated here since they are
aspirations, not current architecture.

**This README was rewritten in the same change that added this
`doc/system/` tree** — the previous README was an unedited, pasted AI-chat
response (including the chat wrapper text itself). See the current
`README.md` for the corrected version.
