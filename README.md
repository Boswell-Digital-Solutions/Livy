# Livy

Livy is a location-aware history-tour application: a map-based frontend
showing points of interest along a tour, paired with a FastAPI backend for
authentication and tour/stop data. The pilot tour is the Lexington Heritage
Loop.

## Documentation Contract

- **Repo type:** Application — SvelteKit frontend + FastAPI backend.
- **Authority boundary:** Livy owns its own tour content, auth, and data model. It has no dependency on other Forge/bds systems.
- **Deep reference:** `doc/system/_index.md` and the assembled `doc/livSYSTEM.md` are the authoritative technical reference.
- **README role:** Entrypoint overview. `doc/system/` is authoritative for architecture, configuration, and implementation-status detail.
- **Truth note:** Feature descriptions below reflect what is actually implemented as of this writing. See `doc/system/01-overview-philosophy.md` for what is planned but not yet built.

## What's implemented

- Map-based tour UI (SvelteKit + `maplibre-gl`), with `about`, `map`, and `tours` routes
- JWT-based authentication (`python-jose`, `passlib`)
- Tour/stop data via SQLAlchemy ORM models
- Object storage integration (`boto3`, presigned URLs)

## Planned, not yet implemented

- **AI-powered Q&A over tour content.** `backend/rag.py` is currently a placeholder that returns a fixed string — it does not call an LLM or perform retrieval. A `pgvector` column type is imported but not yet queried by anything.

## Tech stack

**Backend:** FastAPI, SQLAlchemy, `python-jose`/`passlib` (JWT), `boto3`.
**Frontend:** SvelteKit, `maplibre-gl`, Tailwind CSS.

See `doc/system/03-tech-stack.md` for detail, including what's aspirational versus implemented.

## Getting started

### Frontend

```bash
cd frontend
npm install
npm run dev
```

### Backend

```bash
cd backend
python -m venv venv
source venv/bin/activate
pip install -r requirements.txt
export SECRET_KEY="your-secret-key"   # required — the backend will not start without it
uvicorn main:app --reload
```

By default the backend uses a local SQLite database (`sqlite:///./app.db`).
Set `DATABASE_URL` to a Postgres connection string to use Postgres instead —
see `doc/system/05-config-env.md`.

## Testing

```bash
PYTHONPATH=. pytest
```

## License

MIT License — see [LICENSE](LICENSE).

---

Created by Boswell Web Development Solutions LLC — Lexington, Kentucky.
