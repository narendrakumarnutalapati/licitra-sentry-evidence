# LICITRA-SENTRY Evidence

This repository contains reproducible evidence artifacts for **LICITRA-SENTRY v0.2**, the runtime enforcement layer for AI agent systems that guarantees:

```text
H(authorized_request) = H(executed_request)
```

---

## Canonical Run

The canonical evidence in this repository is the structured run archive under:

```
runs/20260311T043751Z/
```

This run is the validated reference run in which:

- **13** security test checks passed
- **10** runtime security experiments passed
- **3** benchmark validations passed
- Evidence bundles were generated
- A unified evidence manifest was generated

---

## Repository Structure

```
licitra-sentry-evidence/
├── README.md
├── docs/
│   ├── chain-of-intent.md
│   ├── experiments.md
│   └── how_to_run.md
├── runs/
│   └── 20260311T043751Z/
│       ├── tests/
│       │   ├── test_report.json
│       │   └── summary.json
│       ├── experiments/
│       │   ├── summary.json
│       │   ├── EXP-01/
│       │   ├── EXP-02/
│       │   ├── EXP-03/
│       │   ├── EXP-04/
│       │   ├── EXP-05/
│       │   ├── EXP-06/
│       │   ├── EXP-07/
│       │   ├── EXP-08/
│       │   ├── EXP-09/
│       │   └── EXP-10/
│       ├── benchmarks/
│       │   ├── benchmark_report.json
│       │   ├── benchmark_results.json
│       │   └── summary.json
│       ├── evidence/
│       │   ├── index.json
│       │   ├── TEST-001/
│       │   ├── TEST-002/
│       │   ├── EXP-01/
│       │   ├── EXP-02/
│       │   ├── EXP-03/
│       │   ├── EXP-04/
│       │   ├── EXP-05/
│       │   ├── EXP-06/
│       │   ├── EXP-07/
│       │   ├── EXP-08/
│       │   ├── EXP-09/
│       │   ├── EXP-10/
│       │   ├── BENCH-001/
│       │   ├── BENCH-002/
│       │   └── BENCH-003/
│       └── evidence_manifest.json
├── benchmarks_legacy/
└── evidence_legacy/
```

---

## Canonical Evidence Model

The source of truth in this repository is the run archive under `runs/`.

For the validated run `20260311T043751Z`, the top-level evidence manifest is:

```
runs/20260311T043751Z/evidence_manifest.json
```

The evidence manifest records:

- Test record count
- Test check count
- Experiment count
- Benchmark count
- Total record count
- Total validated check count
- Paths to evidence bundles
- Structured evidence details

For this validated run, the manifest counts are:

| Field                      | Count |
| -------------------------- | ----- |
| `test_records`             | 2     |
| `test_checks`              | 13    |
| `experiments`              | 10    |
| `benchmarks`               | 3     |
| `total_records`            | 15    |
| `total_validated_checks`   | 26    |

---

## Validated Test Coverage

The run archive contains two module-level test records:

| Record     | Source                        |
| ---------- | ----------------------------- |
| `TEST-001` | `tests/test_sentry_v02.py`    |
| `TEST-002` | `tests/test_witness.py`       |

These expand into **13 structured security checks**:

| Check | Description                      |
| ----- | -------------------------------- |
| E01   | Authorized Ticket Flow           |
| E02   | Proxy Bypass Attempt             |
| E03   | Replay Attack                    |
| E04   | Payload Modification             |
| E05   | Expired Ticket                   |
| E06   | Delegation Escalation            |
| E07   | PII Exfiltration Blocked         |
| E08   | Audit Chain Integrity            |
| E09   | Epoch Witnessed with Receipt     |
| E10   | Operator Rewrite Detected        |
| E11   | External Auditor Verification    |
| E12   | Tampered Receipt Rejected        |
| E13   | Chain Break Detected             |

---

## Validated Experiment Coverage

The run archive contains **10 runtime security experiments**:

| Experiment | Description                          |
| ---------- | ------------------------------------ |
| EXP-01     | Authorized execution path            |
| EXP-02     | Contract rejection                   |
| EXP-03     | Identity expiry                      |
| EXP-04     | Relay injection attack               |
| EXP-05     | PII exfiltration attempt             |
| EXP-06     | Unauthorized delegation              |
| EXP-07     | End-to-end MMR proof validation      |
| EXP-08     | Ticket replay attack                 |
| EXP-09     | Payload tampering                    |
| EXP-10     | Audit tampering                      |

Each experiment has:

- Raw machine-readable output under `experiments/EXP-xx/experiment_output.json`
- Evidence bundle under `evidence/EXP-xx/evidence.json`
- Human-readable PDF under `evidence/EXP-xx/evidence.pdf`

---

## Benchmarks

The validated run includes **3 benchmark validation records**:

| Benchmark  | Description                |
| ---------- | -------------------------- |
| BENCH-001  | Sequential full pipeline   |
| BENCH-002  | Concurrent full pipeline   |
| BENCH-003  | Security failure checks    |

Benchmark outputs are stored under:

```
runs/20260311T043751Z/benchmarks/
```

---

## Reproducing the Evidence

To regenerate this artifact set:

1. Start **LICITRA-MMR** on port `8000`.
2. Run the LICITRA-SENTRY reproducible pipeline:
   ```bash
   python scripts/run_all.py
   ```
3. Copy the resulting run directory from:
   ```
   artifacts/runs/<run_id>/
   ```
4. Copy the run into this repository under:
   ```
   runs/<run_id>/
   ```

The validated reference run in this repository was produced from:

```
run_id = 20260311T043751Z
```

---

## Legacy Material

The folders below are retained temporarily as legacy material from the earlier evidence-repo organization:

- `benchmarks_legacy/`
- `evidence_legacy/`

They are **not** the canonical source of truth for the current v0.2 reproducible artifact pipeline.

---

## Related Repositories

- [LICITRA-SENTRY](https://github.com/narendrakumarnutalapati/licitra-sentry) — runtime enforcement layer and pipeline source code
- [LICITRA-MMR](https://github.com/narendrakumarnutalapati/licitra-mmr) — append-only cryptographic audit ledger

---

## License

MIT

## Author

Narendra Kumar Nutalapati
