# Day 2 — Project Environment and Reproducible First Run

Date: 2026-11-11  
Project: `ml-infra-2026`  
Objective: Document setup and verify that a new shell can run the starter command using the project environment.

## Work completed

- Installed and verified uv, then initialized the Python project and Git repository.
- Created the virtual environment and lockfile during initial setup.
- Added a [README](../README.md) with the project purpose, setup commands, expected output, directory layout, and evidence links.
- Ran the documented setup and starter command in new Bash login shells without an inherited virtual-environment activation.
- Saved raw command outputs and environment details under `experiments/`.

## Commands and observed results

Run from `~/Desktop/ml`:

| Command | Observed result |
| --- | --- |
| `uv sync --locked` | Exit code 0; resolved and checked the local project without requiring a lockfile update. |
| `uv run --locked ml-infra-2026` | Exit code 0; printed `Hello from ml-infra-2026!`. |
| `uv --version` | uv 0.13.7. |
| `uv run --locked python --version` | Python 3.12.16. |
| `uv run --locked python -c 'import sys; print(sys.executable)'` | `/Users/shangyixiong/Desktop/ml/.venv/bin/python`. The captured environment check also confirmed `sys.prefix != sys.base_prefix`. |

Environment: macOS 13.7.6, build 22H625; architecture `x86_64`. `.python-version` selects the Python 3.12 series. The exact interpreter used for this check is recorded above and in the raw environment log.

## Evidence saved

- [Setup transcript](../experiments/day2-setup.txt): verification timestamp, working directory, exact commands, combined output, and exit codes.
- [Hello-world output](../experiments/day2-hello.txt): the starter command's captured output.
- [Environment inventory](../experiments/day2-environment.txt): uv/Python versions, interpreter path, virtual-environment check, OS, and architecture.
- [Project requirements](../pyproject.toml), [Python selection](../.python-version), and [dependency lockfile](../uv.lock).
- Starter implementation: [src/ml_infra_2026/__init__.py](../src/ml_infra_2026/__init__.py).

The initial install/init commands were completed before this verification; their original terminal output was not captured. The saved transcript records the subsequent verification run.

## Decision and limits

Use uv to manage the project environment. Run project commands with `uv run --locked`; manual activation is optional. Keep the requirements, Python selection, and lockfile in Git; recreate `.venv/` rather than committing it.

The README commands passed in separate new Bash login shells using the existing `.venv/`. This verifies command execution without manual activation. Rebuilding from a clean checkout or on another machine has not yet been tested. The starter application has no runtime dependencies, and no ingestion, training, or performance results are claimed.

## Next action

Save the reviewed files in the first Git commit. Then begin Day 3: generate 100 valid deterministic synthetic scene records plus two malformed examples, with fixed-seed payloads, scene ID, timestamp, sensor type, URI, checksum, and schema version. Check that two same-seed runs produce identical valid records and checksums.
