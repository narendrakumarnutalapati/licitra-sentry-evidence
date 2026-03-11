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
python -m uvicorn app.main:app --host 0.0.0.0 --port 8000
```

Verify the service is reachable:

```bash
curl http://127.0.0.1:8000/health
```

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

The canonical evidence manifest is:

```
runs/20260311T043751Z/evidence_manifest.json
```

Expected counts:

| Field                    | Count |
| ------------------------ | ----- |
| `test_records`           | 2     |
| `test_checks`            | 13    |
| `experiments`            | 10    |
| `benchmarks`             | 3     |
| `total_records`          | 15    |
| `total_validated_checks` | 26    |
