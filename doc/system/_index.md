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
