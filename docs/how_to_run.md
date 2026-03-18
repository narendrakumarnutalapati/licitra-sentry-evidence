# How to Reproduce LICITRA-SENTRY Evidence

## Prerequisites

- Python 3.12+
- LICITRA-MMR running locally on port `8000`
- LICITRA-SENTRY source repository
- Git

---

## Step 1 — Start LICITRA-MMR

```bash
git clone https://github.com/narendrakumarnutalapati/licitra-mmr-core.git
cd licitra-mmr-core
pip install -r requirements.txt
```

Start in experiment mode (recommended for full validation):

**Windows (PowerShell):**

```powershell
.\scripts\switch_env.ps1 -mode experiment
.\scripts\run_server.ps1
```

**For non-Windows environments**, configure LICITRA-MMR manually to:

- `block_size = 2`
- `ledger_mode = experiment`
- `dev_mode = true`

Then start the LICITRA-MMR server using your platform's equivalent command (e.g., `uvicorn app.main:app --host 0.0.0.0 --port 8000`).

Verify the service is reachable:

```bash
curl http://127.0.0.1:8000/health
```

Expected response (experiment mode):

```json
{ "status": "ok", "service": "licitra-mmr", "ledger_version": "mmr-v0.1", "block_size": 2, "ledger_mode": "experiment", "dev_mode": true }
```

If `block_size`, `ledger_mode`, or `dev_mode` differ, the system is not in experiment mode and EXP-07 / EXP-10 will not execute.

> **IMPORTANT:**
>
> For full validation (including EXP-07 and EXP-10), LICITRA-MMR must run in:
>
> - `block_size = 2`
> - `ledger_mode = experiment`
> - `dev_mode = true`
>
> This is what `.\scripts\switch_env.ps1 -mode experiment` configures on Windows.
>
> Otherwise:
>
> - EXP-07 and EXP-10 will be skipped
> - Full cryptographic proof verification will not be performed

---

## Step 2 — Clone and Install LICITRA-SENTRY

In a separate terminal:

```bash
git clone https://github.com/narendrakumarnutalapati/licitra-sentry.git
cd licitra-sentry
pip install -r requirements.txt
```

---

## Step 3 — Run the Full Reproducible Pipeline

Ensure LICITRA-MMR is running before executing the pipeline.

The canonical reproduction path is:

```bash
python scripts/run_all.py
```

This pipeline runs, in order:

1. Security tests
2. Runtime security experiments
3. Benchmark suite
4. Evidence bundle generation
5. Evidence manifest generation

---

## Step 4 — Inspect Output Artifacts

Pipeline artifacts are written under:

```
artifacts/runs/<run_id>/
```

Example structure:

```
artifacts/runs/<run_id>/
├── tests/
├── experiments/
├── benchmarks/
├── evidence/
└── evidence_manifest.json
```

---

## Step 5 — Copy the Run into the Evidence Repository

After a successful run, copy the run directory from:

```
artifacts/runs/<run_id>/
```

into the evidence repository under:

```
runs/<run_id>/
```

---

## Step 6 — Validate the Manifest

The canonical validated run is:

```
runs/20260318T060019Z/evidence_manifest.json
```

The previous run `20260311T043751Z` is retained as historical reference.

The canonical run (20260318T060019Z) completed with:

- **Overall Status:** PASS
- **All tests, experiments, and benchmarks validated successfully**

Expected counts:

| Field                    | Count |
| ------------------------ | ----- |
| `test_records`           | 2     |
| `test_checks`            | 13    |
| `experiments`            | 10    |
| `benchmarks`             | 3     |
| `total_records`          | 15    |
| `total_validated_checks` | 26    |
