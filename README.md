# LICITRA-SENTRY Evidence

This repository contains reproducible evidence artifacts for LICITRA-SENTRY v0.2, a runtime enforcement system for AI agents that guarantees:

```text
H(authorized_request) = H(executed_request)
```

AI agent systems can execute actions that are difficult to verify after the fact. This repository provides cryptographic evidence that every executed action matches its authorized intent, even under adversarial conditions.

The enforcement logic is implemented in LICITRA-SENTRY, and audit commitments are handled by LICITRA-MMR. The artifacts here provide reproducible proof that the combined system enforces the invariant under real execution conditions.

---

## Proof Guarantee

This repository provides verifiable proof that the LICITRA-SENTRY enforcement invariant holds under adversarial conditions. Across all validated tests, experiments, and benchmarks, every executed request is cryptographically bound to its authorization decision, and any deviation (replay, tampering, or unauthorized action) is detected and rejected.

Unlike traditional audit systems that detect issues after execution, LICITRA-SENTRY enforces correctness at runtime. Authorization decisions are cryptographically bound to the exact request executed by a tool, ensuring that any deviation between approved intent and actual execution is prevented rather than merely detected.

---

## Canonical Run

The canonical evidence in this repository is the structured run archive under:

```
runs/20260318T060019Z/
```

This is the current canonical validated run in which:

- **13** security test checks passed
- **10** runtime security experiments passed
- **3** benchmark validations passed
- Evidence bundles were generated
- A unified evidence manifest was generated

The previous run (20260311T043751Z) is retained as a historical reference.

This run provides a complete, reproducible validation of LICITRA-SENTRY under both normal and adversarial execution scenarios.

---

## How to Verify the Evidence

To validate the enforcement guarantee:

1. Open the canonical evidence manifest: `runs/20260318T060019Z/evidence_manifest.json`
2. Confirm aggregate counts:
   - `test_checks = 13`
   - `experiments = 10`
   - `benchmarks = 3`
   - `total_validated_checks = 26`
3. Inspect experiment results:
   - `runs/20260318T060019Z/experiments/summary.json`
   - Verify all experiments report `"status": "PASS"`
4. Validate adversarial scenarios:
   - EXP-08 → replay attack rejected
   - EXP-09 → payload tampering rejected
   - EXP-10 → audit tampering detected
5. Cross-check evidence bundles:
   - `runs/20260318T060019Z/evidence/EXP-xx/evidence.json`
   - Each contains cryptographic linkage between:
     - authorized request
     - execution ticket
     - executed payload
     - audit record
   - The linkage is enforced through:
     - `request_hash` (SHA-256 of authorized payload)
     - `execution_ticket` (Ed25519 signature)
     - `jti` (unique token for replay protection)
6. (Optional) Reproduce the run:
   - Follow `docs/how_to_run.md`
   - Ensure LICITRA-MMR runs in experiment mode

### Failure Conditions

A validation must fail if any of the following occur:

- mismatch between authorized request hash and executed payload
- invalid, expired, or replayed execution ticket
- tampered audit record or broken hash chain

---

## Runtime Context (MMR Mode)

This canonical run was generated with LICITRA-MMR in the following configuration:

- `block_size`: 2
- `ledger_mode`: experiment
- `dev_mode`: True

This configuration enables:

- Immediate epoch finalization
- End-to-end MMR proof validation (EXP-07)
- Audit tampering detection (EXP-10)

In default mode (large block_size), these proofs are not available during short runs and certain experiments are skipped.

As a result, experiments that require real-time inclusion proofs (EXP-07 and EXP-10) cannot be validated during short runs in default mode.

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
│   ├── 20260311T043751Z/ # historical validated run
│   └── 20260318T060019Z/ # current canonical run
├── benchmarks_legacy/
└── evidence_legacy/
```

---

## Canonical Evidence Model

The source of truth in this repository is the run archive under `runs/`.

For the validated canonical run `20260318T060019Z`, the top-level evidence manifest is:

```
runs/20260318T060019Z/evidence_manifest.json
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
runs/20260318T060019Z/benchmarks/
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
run_id = 20260318T060019Z
```

The previous run (20260311T043751Z) is retained as a historical reference.

For step-by-step instructions and environment setup, see:

- `docs/how_to_run.md`
- `docs/experiments.md`

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
