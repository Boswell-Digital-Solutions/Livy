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
