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
