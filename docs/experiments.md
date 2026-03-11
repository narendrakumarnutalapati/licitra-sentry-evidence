# LICITRA-SENTRY Experiments

## Overview

LICITRA-SENTRY v0.2 includes **10 runtime security experiments** designed to validate the enforcement invariant:

```text
H(authorized_request) = H(executed_request)
```

Experiments are executed as part of the reproducible pipeline and archived under a run directory.

Example run in this repository:

```
runs/20260311T043751Z/
```

Each experiment produces:

- **Machine-readable output**
  `experiments/EXP-xx/experiment_output.json`

- **Structured evidence bundle**
  `evidence/EXP-xx/evidence.json`

- **Human-readable report**
  `evidence/EXP-xx/evidence.pdf`

---

## Experiment List

| ID     | Description                          |
| ------ | ------------------------------------ |
| EXP-01 | Authorized execution path            |
| EXP-02 | Contract rejection                   |
| EXP-03 | Identity expiry                      |
| EXP-04 | Relay injection attack               |
| EXP-05 | PII exfiltration attempt             |
| EXP-06 | Unauthorized delegation              |
| EXP-07 | End-to-end MMR proof validation      |
| EXP-08 | Ticket replay attack                 |
| EXP-09 | Payload tampering                    |
| EXP-10 | Audit tampering                      |

---

## Running Experiments

The canonical way to run the experiments is through the pipeline:

```bash
python scripts/run_all.py
```

This runs:

1. Security tests
2. Experiment suite
3. Benchmark suite
4. Evidence generation
5. Evidence manifest generation

---

## Output Structure

Experiment outputs are stored under:

```
runs/<run_id>/experiments/
```

Example:

```
runs/20260311T043751Z/experiments/
├── summary.json
├── EXP-01/experiment_output.json
├── EXP-02/experiment_output.json
├── EXP-03/experiment_output.json
├── EXP-04/experiment_output.json
├── EXP-05/experiment_output.json
├── EXP-06/experiment_output.json
├── EXP-07/experiment_output.json
├── EXP-08/experiment_output.json
├── EXP-09/experiment_output.json
└── EXP-10/experiment_output.json
```

Evidence bundles are stored under:

```
runs/<run_id>/evidence/
```

---

## Canonical Reference Run

The validated reference run archived in this repository is:

```
20260311T043751Z
```

Experiment summary:

```
runs/20260311T043751Z/experiments/summary.json
```

Evidence bundles:

```
runs/20260311T043751Z/evidence/
```
