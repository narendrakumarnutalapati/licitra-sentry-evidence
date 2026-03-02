# LICITRA-SENTRY Experiments

## Overview

Six experiments validate the Chain of Intent pipeline across all 10 OWASP Agentic Top 10 categories. Each experiment is a standalone Python script that can be run independently.

## Experiment Protocol

1. Start LICITRA-MMR at http://localhost:8000
2. Build the SENTRY stack (identity, contracts, authority, inspector, orchestration, audit bridge, middleware)
3. Execute scenario through the Chain of Intent pipeline
4. Capture the MiddlewareResult including MMR leaf_hash
5. Verify: decision matches expected, gate matches expected, leaf_hash is 64-char SHA-256 hex
6. Verdict: CONFIRMED if all checks pass

## Experiments

### EXP-01: Happy Path — Approved

**Script:** `experiments/run_exp01_happy_path.py`
**Goal:** Valid agent, valid token, allowed intent, allowed tool, clean message passes all gates.
**Agent:** Researcher | **Intent:** READ | **Tool:** web_search
**Expected:** APPROVED, gate=approved, leaf_hash present
**OWASP:** ASI07 (Inter-Agent Communication Integrity)

### EXP-02: Contract Rejection — Excessive Agency Blocked

**Script:** `experiments/run_exp02_contract_rejection.py`
**Goal:** Contract engine rejects intent not in agent's allowed list.
**Agent:** Researcher | **Intent:** FILE_WRITE (not allowed) | **Tool:** editor
**Expected:** REJECTED, gate=contract, leaf_hash present
**OWASP:** ASI02 (Excessive Agency)

### EXP-03: Identity Expiry Rejection — Impersonation Blocked

**Script:** `experiments/run_exp03_identity_expiry.py`
**Goal:** Expired token rejected at identity gate.
**Agent:** Researcher (expired token) | **Intent:** READ
**Expected:** REJECTED, gate=identity, leaf_hash present
**OWASP:** ASI03 (Agent Impersonation)

### EXP-04: Relay Injection Blocked

**Script:** `experiments/run_exp04_relay_injection.py`
**Goal:** Content inspector detects relay injection pattern.
**Agent:** Researcher | **Message:** "ignore all previous instructions..."
**Expected:** REJECTED, gate=inspector, findings include RI-001, leaf_hash present
**OWASP:** ASI01 (Prompt Injection / Relay Injection)

### EXP-05: PII Exfiltration Blocked — SSN Detection

**Script:** `experiments/run_exp05_pii_exfiltration.py`
**Goal:** Content inspector detects US SSN pattern.
**Agent:** Researcher | **Message:** contains "123-45-6789"
**Expected:** REJECTED, gate=inspector, findings include PII-001, leaf_hash present
**OWASP:** ASI06 (Sensitive Data Exposure)

### EXP-06: Unauthorized Delegation Blocked — Orchestration Guard

**Script:** `experiments/run_exp06_unauthorized_delegation.py`
**Goal:** Orchestration guard blocks unauthorized agent delegation.
**Agent:** Coder delegates to Researcher (policy only allows Researcher->Coder)
**Expected:** REJECTED, gate=orchestration, leaf_hash present
**OWASP:** ASI05 (Improper Multi-Agent Orchestration)

## Reproducibility

All experiments are deterministic given the same inputs. The MMR leaf_hash will differ between runs (timestamps vary), but decision, gate_fired, and findings are identical.

Evidence PDFs in the `evidence/` folder capture one complete run with real MMR leaf_hashes.
