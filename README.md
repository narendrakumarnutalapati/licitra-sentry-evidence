# LICITRA-SENTRY Evidence

Reproducible experiments and cryptographic evidence bundles for [LICITRA-SENTRY](https://github.com/narendrakumarnutalapati/licitra-sentry).

## Contents
```
licitra-sentry-evidence/
├── evidence/
│   ├── EXP-01_Happy_Path_Approved.pdf
│   ├── EXP-02_Contract_Rejection.pdf
│   ├── EXP-03_Identity_Expiry_Rejection.pdf
│   ├── EXP-04_Relay_Injection_Blocked.pdf
│   ├── EXP-05_PII_Exfiltration_Blocked.pdf
│   ├── EXP-06_Unauthorized_Delegation_Blocked.pdf
│   └── LICITRA_SENTRY_Consolidated_Evidence.pdf
├── docs/
│   ├── how_to_run.md
│   ├── experiments.md
│   └── chain-of-intent.md
├── README.md
├── SECURITY.md
└── LICENSE
```

## Evidence Reports

Each experiment PDF is self-contained — includes hypothesis, setup, input, expected vs actual outcome, LICITRA-MMR cryptographic proof (staged_id, event_id, leaf_hash), inspection findings, raw JSON, and verdict.

| Report | Experiment | OWASP | Verdict |
|--------|-----------|-------|---------|
| EXP-01 | Happy Path — Approved | ASI07 | CONFIRMED |
| EXP-02 | Contract Rejection — Excessive Agency | ASI02 | CONFIRMED |
| EXP-03 | Identity Expiry — Impersonation Blocked | ASI03 | CONFIRMED |
| EXP-04 | Relay Injection Blocked | ASI01 | CONFIRMED |
| EXP-05 | PII Exfiltration — SSN Detection | ASI06 | CONFIRMED |
| EXP-06 | Unauthorized Delegation Blocked | ASI05 | CONFIRMED |

The **Consolidated Evidence Report** combines all 6 experiments plus test suite results (9/9), OWASP coverage (10/10), and competitive comparison vs Oktsec.

## Cryptographic Verification

Every experiment produces an MMR `leaf_hash` — a 64-character SHA-256 hex string committed to LICITRA-MMR's Merkle Mountain Range. This hash can be independently verified against the LICITRA-MMR ledger to prove the decision was recorded and has not been tampered with.

## Reproducing

1. Start LICITRA-MMR: `cd licitra-mmr-core && python -m uvicorn app.main:app --host 0.0.0.0 --port 8000`
2. Clone LICITRA-SENTRY: `git clone https://github.com/narendrakumarnutalapati/licitra-sentry.git`
3. Install: `cd licitra-sentry && pip install -r requirements.txt`
4. Run individual experiment: `python experiments/run_exp01_happy_path.py`
5. Run all experiments: `python experiments/run_all_experiments.py`
6. Run full test suite: `powershell -ExecutionPolicy Bypass -File tests\run_all_tests.ps1`

See [docs/how_to_run.md](docs/how_to_run.md) for detailed instructions.

## Related

- [LICITRA-SENTRY](https://github.com/narendrakumarnutalapati/licitra-sentry) — Zero-Trust Control Plane (source code + experiment scripts)
- [LICITRA-MMR](https://github.com/narendrakumarnutalapati/licitra-mmr-core) — Cryptographic Integrity Layer

## License

MIT — see [LICENSE](LICENSE).

## Author

Narendra Kumar Nutalapati
