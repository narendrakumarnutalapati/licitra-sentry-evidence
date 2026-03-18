# LICITRA-SENTRY Experiments

## Overview

LICITRA-SENTRY v0.2 includes **10 runtime security experiments** designed to validate the enforcement invariant:

```text
H(authorized_request) = H(executed_request)
```

Experiments are executed as part of the reproducible pipeline and archived under a run directory.

The current validated canonical run archived in this repository is:

```
runs/20260318T060019Z/
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
runs/20260318T060019Z/experiments/
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

## Execution Modes

LICITRA-SENTRY experiments operate in two modes depending on LICITRA-MMR configuration. The active mode is determined by the running LICITRA-MMR instance at execution time.
The experiment pipeline does not override this configuration; it directly reflects the active MMR runtime mode.

### Default Mode

- `block_size`: large (e.g., 1000)
- `ledger_mode`: default
- EXP-07 and EXP-10 will be **skipped** because MMR blocks do not finalize until 1000 events are committed, so inclusion proofs are not available during a short experiment run.

### Experiment Mode

- `block_size`: 2
- `ledger_mode`: experiment
- **Required** for:
  - EXP-07 (End-to-end MMR proof validation)
  - EXP-10 (Audit tampering detection)

> **Note:** The canonical run in this repository was generated in experiment mode. This is the recommended configuration for full validation.

Enforcement correctness is independent of MMR mode; only proof availability is mode-dependent.

---

## Canonical Reference Run

The canonical run referenced above is:

```
20260318T060019Z
```

The previous run `20260311T043751Z` is retained as historical reference.

Experiment summary:

```
runs/20260318T060019Z/experiments/summary.json
```

Evidence bundles:

```
runs/20260318T060019Z/evidence/
```
