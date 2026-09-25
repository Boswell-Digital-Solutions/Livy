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
