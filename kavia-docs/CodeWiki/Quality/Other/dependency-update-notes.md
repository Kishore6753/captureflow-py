# Dependency update notes (conservative)

Date: 2026-02-20  
Scope: `captureflow-py` monorepo (clientside / serverside / clientside_v2)

## What changed

### clientside (pip requirements)
- Regenerated `clientside/requirements.txt` using `pip-tools` (`pip-compile`) with conservative upgrades.
- Notable bumps include:
  - `httpx` 0.27.0 → 0.28.1
  - `requests` 2.31.0 → 2.32.5
  - `urllib3` 2.2.1 → 2.6.3
  - `certifi` 2024.2.2 → 2026.1.4
  - plus transitive updates (`anyio`, `httpcore`, `h11`, etc.)

### clientside_v2 (Poetry)
- Regenerated `clientside_v2/poetry.lock` using Poetry and refreshed locked versions within existing declared ranges.
- `clientside_v2/pyproject.toml` dependency ranges were intentionally left unchanged to avoid accidental breaking major bumps.

## Notes / caveats

- This workspace environment includes preinstalled packages (notably `shelly-ai` and `litellm`) that **pin** certain versions (e.g., `filelock==3.17.0`, `httpx<0.28`, and newer `openai`). These constraints can produce pip resolver warnings when installing this repo's requirements into the shared environment.
- The project’s pinned dependency sets remain internally consistent; resolver warnings are attributable to the shared environment rather than the repo’s declared requirements.
- Quick validation performed:
  - `python -c "import fastapi, pydantic, httpx, requests, redis, openai"` (imports succeeded)
  - `cd clientside_v2 && poetry run python -c "import captureflow, opentelemetry"` (imports succeeded)

## Follow-ups (optional)
- If CI/build uses an isolated venv (recommended), the above environment conflicts should not apply.
- If you want to modernize `serverside/requirements*.txt` similarly, consider introducing `requirements.in` / `requirements-dev.in` sources so pip-tools can recompile without “input == output” issues.
