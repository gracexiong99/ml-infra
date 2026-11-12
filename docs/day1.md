# Day 1 — Project Scope and Environment

Date: 2026-11-11  
Project: A small, reproducible sensor-data ingestion and training platform.

## Three gaps I want evidence for

These are evidence targets for the project; implementation and measurements are still pending.

| Gap | Evidence I will collect |
| --- | --- |
| Distributed training | A single-process baseline and a two-process CPU/Gloo training run, with commands, ranks, data partitioning, timing, and a worker-failure/recovery log. Record checkpoint contents and what resumes. Keep any performance claims within the measured hardware scope. |
| End-to-end data ingestion and training | One documented workflow from deterministic synthetic sensor payloads through validation, a PostgreSQL registry, and an immutable dataset manifest to a training run and saved model. Record input checksums, dataset version, code version, configuration, and model lineage. |
| Post-training evaluation and automatic retraining | A repeatable evaluation command and two bounded new-data/retraining cycles. Save fixed evaluation manifests, metrics, trigger reasons, candidate/incumbent comparisons, promotion or rejection decisions, and rollback/retry results. Evaluate the selected model once on an untouched final-test split. |

## Scope

**Input:** Start with 100 valid synthetic scene records and two deliberately malformed examples. Each valid record has a scene ID, timestamp, sensor type, payload URI, checksum, and schema version. Use a fixed seed and keep initial payloads to roughly 100 MB. Add a deterministic classification label when training begins.

**Workflow:** Generate scenes → validate and ingest → register metadata → publish a versioned dataset manifest → train a small model → evaluate → trigger bounded retraining when a new training batch arrives → compare and promote or reject the candidate.

**Output:** Accepted/rejected counts and reasons, registry records, immutable manifests, model/checkpoint artifacts, evaluation reports, and logs linking each result to its inputs and configuration.

**Implementation boundary:** Build incrementally in one repository. Start with Python and a filesystem-backed object store; add PostgreSQL, a job API, Kubernetes workers, and PyTorch as their sessions arrive. Use local CPU workloads first. A two-process laptop run demonstrates distributed execution mechanics; GPU or multi-host measurements require separate hardware evidence.

## Expected failures and success measures

| Case | Completion evidence |
| --- | --- |
| Reproducibility | Same-seed generation produces identical valid records and checksums; a fresh terminal can run the documented setup and hello-world command. |
| Invalid, missing, or corrupt input | Malformed metadata, missing payloads, and checksum mismatches are detected with recorded reasons. |
| Duplicate, interrupted, or concurrent ingestion | Repeated submissions produce one logical result; interruption exposes no ready partial output; a resumed backfill finishes remaining work. |
| Training interruption | A saved checkpoint restores the documented model, optimizer, progress, and data-resumption state. A worker-loss exercise records the failure and recovery outcome. |
| Evaluation and retraining | Fixed model/input evaluations agree within a documented tolerance. Two cycles record lineage and decisions; repeated triggers create no duplicate logical jobs. Validation drives promotion; the final-test split remains untouched until the final report. |

## Environment inventory

Observed on 2026-11-11 from the project terminal:

| Item | Observed result | Command/source |
| --- | --- | --- |
| Laptop | MacBook Pro, MacBookPro14,1; Intel Core i5, 2.3 GHz, 2 cores | `system_profiler SPHardwareDataType` |
| OS | macOS 13.7.6, build 22H625 | `sw_vers` |
| Architecture | `x86_64` | `uname -m` |
| RAM | 8 GB | `system_profiler SPHardwareDataType` |
| Available disk | About 9.0 GiB free on the project volume; changes over time | `df -h ~/Desktop/ml` |
| Existing Python | Python 3.7.2 at `/Library/Frameworks/Python.framework/Versions/3.7/bin/python3` | `command -v python3`; `python3 --version` |
| uv | Command not found in this terminal | `command -v uv`; `uv --version` |
| Containers | Docker and Docker Compose commands not found; runtime compatibility and operation not yet verified | `docker --version`; `docker compose version` |
| Project layout | `src/`, `tests/`, `fixtures/`, `deploy/`, `experiments/`, `docs/` exist | Local directory inspection |

## Day 1 evidence and next action

**Evidence saved:** This scope note, the three evidence targets, and the observed environment inventory. No pipeline, training, or evaluation outcomes have been measured yet.

**Decision:** Keep the initial workload small and CPU-based. Set up a project-specific Python environment during Day 2, and verify container compatibility before container sessions. Account for the available disk space before downloading toolchains or images.

**Next action — Day 2:** Install uv and select a supported project Python after checking the current installation guidance and Mac compatibility. Initialize Git and the Python project; add a lockfile, hello-world command, README setup steps, and experiment log. Verify the command from a fresh terminal and save its command and output as Day 2 evidence.
