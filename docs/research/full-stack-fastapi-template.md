# Full Stack FastAPI Template Layout Review

**Status:** Completed implementation-layout reference

**Reviewed:** 2026-09-02

**Source revision:** [`cb740b6`](https://github.com/fastapi/full-stack-fastapi-template/tree/cb740b656d7a0a6c5e12c7bf8e50343ec94ee9c7)

## Why This Source Matters

[`fastapi/full-stack-fastapi-template`](https://github.com/fastapi/full-stack-fastapi-template)
is the public full-stack template published in the FastAPI GitHub organization.
Its README identifies FastAPI as the Python API, React as the frontend,
PostgreSQL as the database, and Docker Compose as the local and self-hosted
orchestration layer. This review uses the repository tree and documentation at
the immutable revision above rather than treating the moving `master` branch as
stable evidence.

This source informs the recommended **implementation layout** for a full-stack
application with a Python backend. It does not replace this repository's
specification, decision, or documentation layout.

## Layout Summary

```text
.
├── backend/                         # Independently configured Python service
│   ├── app/                         # Importable application package
│   │   ├── alembic/                 # Database migration environment/revisions
│   │   ├── api/
│   │   │   ├── routes/              # Endpoint modules grouped by capability
│   │   │   ├── deps.py              # Shared API dependencies
│   │   │   └── main.py              # API router composition
│   │   ├── core/                    # Configuration, database, and security
│   │   ├── email-templates/          # Generated runtime email HTML
│   │   ├── main.py                  # FastAPI application composition
│   │   ├── models.py                # SQL/data models
│   │   ├── crud.py                  # Persistence operations
│   │   ├── initial_data.py          # Initial database data
│   │   └── utils.py                 # Backend utilities
│   ├── scripts/                     # Backend setup, lint, and test entry points
│   ├── tests/                       # API, CRUD, script, and test utility suites
│   ├── Dockerfile
│   ├── alembic.ini
│   └── pyproject.toml               # Backend package and tool configuration
├── frontend/                        # Independently configured web application
│   ├── public/                      # Static assets
│   ├── src/
│   │   ├── client/                  # Generated client from backend OpenAPI
│   │   ├── components/              # Product and reusable UI components
│   │   ├── hooks/                   # Reusable React hooks
│   │   ├── lib/                     # Shared frontend utilities
│   │   └── routes/                  # Pages and route tree
│   ├── tests/                       # Playwright end-to-end tests
│   └── package/config files         # Vite, TypeScript, UI, and test config
├── packages/
│   └── react-email/                 # Source email components shared by tooling
├── scripts/                         # Cross-stack generation/test/release tasks
├── .github/workflows/               # Backend, Compose, browser, and deploy CI/CD
├── compose.yml                      # Shared service topology
├── compose.override.yml             # Local development specialization
├── compose.deploy.yml               # Deployment specialization
├── pyproject.toml                   # Root Python/uv workspace configuration
├── package.json                     # Root JS workspace and command façade
├── uv.lock / bun.lock               # Reproducible backend/frontend dependencies
└── .env and repository config       # Cross-stack environment and tooling
```

## Structural Responsibilities

1. **One repository, explicit application boundaries.** `backend/` and
   `frontend/` are first-class roots with their own dependencies, tests,
   containers, and development documentation. Root files coordinate the stack;
   they do not hold application business logic.
2. **One importable backend package.** Runtime Python code lives in
   `backend/app/`. Application assembly is in `app/main.py`; API router assembly
   is in `app/api/main.py`; endpoint modules are below `app/api/routes/`; stable
   cross-cutting concerns are below `app/core/`.
3. **Backend lifecycle stays with the backend.** Alembic migrations, startup
   scripts, backend tests, Python packaging, and the backend image all live
   below `backend/`.
4. **The frontend is organized by runtime role.** Generated API bindings,
   components, hooks, shared utilities, routes, static assets, and browser tests
   have distinct homes below `frontend/`.
5. **The API contract connects the two applications.** FastAPI emits OpenAPI;
   `scripts/generate-client.sh` generates `frontend/src/client/`. The template
   instructs maintainers to regenerate and commit the client when the API schema
   changes.
6. **Generated artifacts have named sources.** React Email source lives in
   `packages/react-email/`; rendered HTML lives in
   `backend/app/email-templates/` and is explicitly not hand-edited. The built
   frontend is likewise written into `backend/app/frontend/` for FastAPI to
   serve, but is excluded from version control.
7. **Orchestration is layered at the root.** The shared Compose topology is
   specialized by development and deployment files. Root workspace manifests,
   lockfiles, scripts, environment configuration, and CI operate across
   component boundaries.
8. **Tests follow the owning component.** Pytest coverage is under
   `backend/tests/`; Playwright end-to-end coverage is under `frontend/tests/`;
   GitHub Actions provides separate backend, Compose, and browser workflows.

## Adopted Guidance

For a full-stack application with a Python backend, this is the right default
layout: keep `backend/` and `frontend/` as independent application roots, keep
cross-stack orchestration at the repository root, place shared build-time
packages in `packages/`, and make generated API/runtime artifacts explicit.
Start from these boundaries for a new project and preserve them in an existing
project.

Copy the **responsibilities and boundaries**, not every sample technology or
file name. A project may replace React, Bun, SQLModel, PostgreSQL, Alembic,
Traefik, or React Email while retaining the layout. A framework that imposes a
different internal Python package structure may specialize `backend/app/`.
Diverge from the top-level boundary only when an approved constraint or ADR
records why another structure is better for that project.

## Evidence

The following upstream sources were inspected at revision `cb740b6`:

- [Repository tree and README](https://github.com/fastapi/full-stack-fastapi-template/tree/cb740b656d7a0a6c5e12c7bf8e50343ec94ee9c7)
- [`backend/README.md`](https://github.com/fastapi/full-stack-fastapi-template/blob/cb740b656d7a0a6c5e12c7bf8e50343ec94ee9c7/backend/README.md)
- [`backend/app/`](https://github.com/fastapi/full-stack-fastapi-template/tree/cb740b656d7a0a6c5e12c7bf8e50343ec94ee9c7/backend/app)
- [`frontend/README.md`](https://github.com/fastapi/full-stack-fastapi-template/blob/cb740b656d7a0a6c5e12c7bf8e50343ec94ee9c7/frontend/README.md)
- [`development.md`](https://github.com/fastapi/full-stack-fastapi-template/blob/cb740b656d7a0a6c5e12c7bf8e50343ec94ee9c7/development.md)
- [`compose.yml`](https://github.com/fastapi/full-stack-fastapi-template/blob/cb740b656d7a0a6c5e12c7bf8e50343ec94ee9c7/compose.yml)
- [`pyproject.toml`](https://github.com/fastapi/full-stack-fastapi-template/blob/cb740b656d7a0a6c5e12c7bf8e50343ec94ee9c7/pyproject.toml)
- [`package.json`](https://github.com/fastapi/full-stack-fastapi-template/blob/cb740b656d7a0a6c5e12c7bf8e50343ec94ee9c7/package.json)
- [`scripts/generate-client.sh`](https://github.com/fastapi/full-stack-fastapi-template/blob/cb740b656d7a0a6c5e12c7bf8e50343ec94ee9c7/scripts/generate-client.sh)

## Limits

The source is an opinionated production starter, not evidence that every
technology choice is universally optimal. Its example backend begins with
single `models.py`, `crud.py`, and `utils.py` modules; larger systems may split
those by domain without weakening the top-level component boundary. Its
authentication, deployment, and sample domain are implementation examples, not
requirements inherited by projects that adopt the layout.
