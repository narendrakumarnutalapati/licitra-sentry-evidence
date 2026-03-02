# How to Run LICITRA-SENTRY

## Prerequisites

- Python 3.12+
- PostgreSQL 16 (for LICITRA-MMR)
- Git

## Step 1: Clone and Start LICITRA-MMR
```bash
git clone https://github.com/narendrakumarnutalapati/licitra-mmr-core.git
cd licitra-mmr-core
pip install -r requirements.txt
python -m uvicorn app.main:app --host 0.0.0.0 --port 8000
```

Verify MMR is running: visit http://localhost:8000/docs

## Step 2: Clone and Install LICITRA-SENTRY

In a separate terminal:
```bash
git clone https://github.com/narendrakumarnutalapati/licitra-sentry.git
cd licitra-sentry
pip install -r requirements.txt
```

## Step 3: Run Individual Experiments

Each experiment is a standalone Python script in the `experiments/` folder:
```bash
python experiments/run_exp01_happy_path.py
python experiments/run_exp02_contract_rejection.py
python experiments/run_exp03_identity_expiry.py
python experiments/run_exp04_relay_injection.py
python experiments/run_exp05_pii_exfiltration.py
python experiments/run_exp06_unauthorized_delegation.py
```

Each script:
- Sets up the agent, contract, and token
- Runs the scenario through the Chain of Intent pipeline
- Commits the decision to LICITRA-MMR
- Prints decision, gate fired, MMR leaf_hash, and verdict

## Step 4: Run All Experiments
```bash
python experiments/run_all_experiments.py
```

Expected output: `ALL 6/6 EXPERIMENTS PASSED`

## Step 5: Run Demo Swarm
```bash
python demo_swarm.py
```

Runs all 6 scenarios and prints a summary table with MMR leaf_hash per scenario.

| Scenario | Agent | Intent | Expected | OWASP |
|----------|-------|--------|----------|-------|
| S1 | Researcher | READ | APPROVED | ASI07 |
| S2 | Researcher | FILE_WRITE | REJECTED (contract) | ASI02 |
| S3 | Researcher | READ (expired token) | REJECTED (identity) | ASI03 |
| S4 | Researcher | READ (relay injection) | REJECTED (inspector) | ASI01 |
| S5 | Researcher | SUMMARIZE (SSN in msg) | REJECTED (inspector) | ASI06 |
| S6 | Coder | FILE_WRITE (delegate) | REJECTED (orchestration) | ASI05 |

## Step 6: Run Full Test Suite
```powershell
powershell -ExecutionPolicy Bypass -File tests\run_all_tests.ps1
```

Expected: 9/9 tests passing.

## Step 7: Verify Evidence

The `evidence/` folder in this repo contains PDF reports from a complete run.
Each PDF is self-contained with:
- Hypothesis, setup, input
- Expected vs actual outcome
- MMR cryptographic proof (staged_id, event_id, leaf_hash)
- Inspection findings (if any)
- Raw JSON output
- Verdict: CONFIRMED

The `leaf_hash` in each PDF is a real SHA-256 hash from LICITRA-MMR's
Merkle Mountain Range. It can be independently verified against the MMR ledger.

## Step 8: Regenerate Evidence PDFs (Optional)

To regenerate the evidence PDFs from a fresh run, use the evidence generator
script in the SENTRY repo (requires reportlab):
```bash
pip install reportlab
python _gen_all_evidence.py
```

This produces 6 individual experiment PDFs + 1 consolidated PDF in the `evidence/` folder.
