# Testing

**Canonical fact:** the backend test suite is a single file,
`tests/test_app.py`. Run it with `pytest` from the repository root (the
`PYTHONPATH=. pytest` form is needed if `backend` is not otherwise
importable — see the repository README's troubleshooting section).

**Not yet established here:** no frontend test tooling (e.g. Vitest,
Playwright) was located while authoring this chapter. If frontend tests
exist, they were not found under `frontend/`; treat frontend changes as
manually verified until a test runner is confirmed.
