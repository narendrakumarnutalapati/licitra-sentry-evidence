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


## MMR Paper Future Scope Fulfillment

The LICITRA-MMR paper (ACNS-ISC 2026, Submission 10) identified four limitations in Section 10. LICITRA-SENTRY directly addresses two of them:

| MMR Paper Section | Limitation | Addressed By | Status |
|------------------|-----------|-------------|--------|
| 10.1 | Float Normalization Gap (RFC 8785 S3.2.2) | LICITRA-MMR v1.1 (planned) | Future |
| 10.2 | Unsigned Epoch Roots (Ed25519 signing) | SENTRY `app/identity.py` - Ed25519 CovenantNotary | Implemented |
| 10.3 | Single-Operator Trust (multi-party witnessing) | Multi-party witnessing protocol (planned) | Future |
| 10.4 | Pre-Execution Integrity (semantic contracts) | SENTRY Chain of Intent - full pipeline | Implemented |

**Section 10.2 - Ed25519 Identity:** The MMR paper noted epoch hashes were not signed. SENTRY introduces Ed25519 cryptographic signing at the agent session level via the CovenantNotary.

**Section 10.4 - Chain of Intent:** The MMR paper explicitly identified SENTRY as the solution: LICITRA-SENTRY intercepts agent actions before they reach the commit pipeline, evaluates them against declarative per-agent semantic contracts, and binds the authorization decision to the MMR record in the same transaction. This is exactly what the Chain of Intent pipeline implements with commitment-before-execution and cryptographic evidence of policy evaluation.

## License

MIT — see [LICENSE](LICENSE).

## Author

Narendra Kumar Nutalapati
