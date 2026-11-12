# ML Infrastructure Learning Project

A small sensor-data ingestion and training platform, built incrementally through a 94-day learning plan.

The project aims to collect evidence for three skills:

- Distributed training and recovery from worker failures.
- End-to-end data ingestion, dataset versioning, and model training.
- Post-training evaluation and automatic retraining with recorded promotion decisions.

## Current status

Day 2 establishes the Python project and a working hello-world command. The data pipeline, training workflow, and evaluation/retraining cycles are planned work. Start with deterministic synthetic data, roughly 100 MB of initial payloads, and local CPU workloads.

## Setup and first run

Prerequisite: install [uv](https://docs.astral.sh/uv/getting-started/installation/) and confirm that `uv --version` works in your terminal. The project selects Python 3.12 through `.python-version`; uv can download a matching interpreter if needed.

Open a new Terminal window and run:

```bash
cd ~/Desktop/ml
uv sync --locked
uv run --locked ml-infra-2026
```

If you place the repository elsewhere, change the first command to that location.

Expected application output:

```text
Hello from ml-infra-2026!
```

`uv sync --locked` creates or updates `.venv/` using `uv.lock` and installs this project. `--locked` fails if the lockfile needs an update. `uv run` uses the project environment automatically, so manual activation is optional.

To check the Python version and interpreter location:

```bash
uv run --locked python --version
uv run --locked python -c 'import sys; print(sys.executable)'
```

The interpreter path should point inside this repository's `.venv/` directory.

## Project layout

| Path | Purpose |
| --- | --- |
| `src/ml_infra_2026/` | Application code; currently the starter `main()` function. |
| `tests/` | Future correctness and recovery tests. |
| `fixtures/` | Future deterministic sample data and malformed examples. |
| `deploy/` | Future container and Kubernetes configuration. |
| `experiments/` | Commands, raw outputs, and experiment measurements. |
| `docs/` | Daily scope notes, decisions, and evidence summaries. |
| `pyproject.toml` | Project metadata, dependency requirements, and command definition. |
| `.python-version` | Python version selected for the project environment. |
| `uv.lock` | Resolved dependencies used to recreate the package environment. |

Empty directories do not appear in Git until files are added to them.

## Environment and dependencies

Keep `pyproject.toml`, `.python-version`, and `uv.lock` in Git. The generated `.venv/` directory is ignored and can be recreated with `uv sync --locked`.

When a later session requires a library, use `uv add PACKAGE_NAME` to update the project requirements, lockfile, and environment together. The starter application currently has no runtime dependencies.

## Learning evidence

- [Day 1: scope and environment inventory](docs/day1.md)
- [Day 2: setup verification and next action](docs/day2.md)

Record the command, actual result, decision, and next action for each session. Keep measured outcomes separate from future targets. Day 2 raw outputs live in `experiments/day2-setup.txt`, `experiments/day2-environment.txt`, and `experiments/day2-hello.txt`.

Next: generate deterministic synthetic scenes and verify that repeated runs produce identical records and checksums.
