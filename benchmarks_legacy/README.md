# LICITRA-SENTRY Benchmark Evidence Bundle

## Overview

This evidence bundle contains reproducible benchmark results for the LICITRA-SENTRY paper:
**"LICITRA-SENTRY: Zero-Trust Inter-Agent Communication Control Plane"**

All benchmarks were run on the actual LICITRA-SENTRY codebase using real cryptographic
operations (Ed25519 signing/verification, SHA-256 hashing, threaded concurrency).

## Environment

- **Python**: 3.12.7 (MSC v.1941 64-bit AMD64)
- **OS**: Windows 11
- **Hardware**: Consumer-grade desktop (AMD64)
- **Date**: March 2, 2026

## Contents

| File | Description |
|------|-------------|
| `benchmark_suite.py` | Complete benchmark script (runs all 3 benchmarks) |
| `benchmark_results.json` | Raw JSON output from benchmark run |
| `benchmark_output.txt` | Console output from benchmark execution |
| `LICITRA_SENTRY_Paper_FINAL.pdf` | Paper with real measured numbers |

## Benchmark Summary

### B1: Concurrent 100-Agent Mixed Workload

| Threads | RPS | p50 (ms) | p95 (ms) | p99 (ms) |
|---------|-----|----------|----------|----------|
| 1 (seq) | 3,669 | 0.284 | 0.392 | 0.713 |
| 10 | 3,124 | 0.290 | 0.483 | 18.329 |
| 20 | 2,742 | 0.326 | 0.629 | 50.342 |
| 50 | 2,743 | 0.317 | 0.642 | 20.576 |

Decision mix: 72.3% approved, 27.7% rejected (70/15/10/5 workload split)

### B2: Signed Epoch Head Prototype

| Metric | Value |
|--------|-------|
| Epochs finalized | 5 (500 events) |
| All heads verified | Yes (5/5) |
| Tamper detection | DETECTED |
| Ed25519 sign overhead | 0.041 ms/epoch |
| Verification | 0.181 ms/head |

### B3: Failure Injection Experiments

| ID | Scenario | Result |
|----|----------|--------|
| F1 | MMR unavailable (fail-closed) | PASS |
| F2 | Leaf corruption detection | PASS |
| F3 | Event deletion detection | PASS |
| F4 | Token replay within TTL | PASS (known limitation) |
| F5 | 100-thread contention | PASS (500/500, 0 errors, 0.226s) |

## Reproduction

```bash
cd licitra-sentry
pip install -r requirements.txt
python experiments/benchmark_suite.py
```

No external services required (uses local MMR simulation with real crypto).

## Related Repositories

- [licitra-sentry](https://github.com/narendrakumarnutalapati/licitra-sentry) — Source code
- [licitra-mmr-core](https://github.com/narendrakumarnutalapati/licitra-mmr-core) — MMR integrity layer
- [licitra-sentry-evidence](https://github.com/narendrakumarnutalapati/licitra-sentry-evidence) — This bundle + experiment evidence

## Author

Narendra Kumar Nutalapati
