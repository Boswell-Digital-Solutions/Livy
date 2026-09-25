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
