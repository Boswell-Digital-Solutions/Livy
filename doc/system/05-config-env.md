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
