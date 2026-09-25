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
