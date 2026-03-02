# Chain of Intent — Formal Specification

## Overview

The Chain of Intent is the core security model of LICITRA-SENTRY. It defines a sequential pipeline of cryptographically enforced gates that every inter-agent message must pass through before being forwarded.

## Definition

A Chain of Intent is a tuple C = (G1, G2, G3, G4, G5, A) where:

- **G1 (Identity Gate):** Verifies the agent's Ed25519-signed session token. Checks signature validity and token expiry.
- **G2 (Content Gate):** Inspects message content against a deterministic rule set for injection patterns, PII, and privilege escalation.
- **G3 (Contract Gate):** Validates that the requested intent, tool, and parameter shapes are permitted by the agent's safety contract.
- **G4 (Authority Gate):** Performs final authorization combining identity validity with contract compliance.
- **G5 (Orchestration Gate):** If the message involves delegation, verifies the delegation is authorized and does not escalate privileges.
- **A (Anchor):** Commits the decision (APPROVED or REJECTED) to LICITRA-MMR via 2-phase commit.

## Sequential Enforcement
```
Message M from Agent X
        |
        v
   +---------+    REJECT --> Anchor(REJECTED, reason=identity) --> MMR
   | G1: ID  |----------->
   +----+----+
        | PASS
        v
   +---------+    REJECT --> Anchor(REJECTED, reason=inspector) --> MMR
   |G2:Content|---------->
   +----+----+
        | PASS
        v
   +---------+    REJECT --> Anchor(REJECTED, reason=contract) --> MMR
   |G3:Contract|--------->
   +----+----+
        | PASS
        v
   +---------+    REJECT --> Anchor(REJECTED, reason=authority) --> MMR
   |G4:Authority|-------->
   +----+----+
        | PASS
        v
   +---------+    REJECT --> Anchor(REJECTED, reason=orchestration) --> MMR
   |G5:Orchestr|-------->   (only if delegate_to is set)
   +----+----+
        | PASS
        v
   Anchor(APPROVED) --> MMR
        |
        v
   Forward Message M
```

## Properties

### P1: Completeness
Every message receives a decision. There is no path through the pipeline that does not produce either APPROVED or REJECTED.

### P2: Tamper Evidence
Every decision is committed to LICITRA-MMR. The MMR leaf_hash provides cryptographic proof that the decision was recorded. Epoch anchoring provides proof of ledger state at any point in time.

### P3: Short-Circuit Safety
The pipeline short-circuits on the first rejection. The rejection reason identifies exactly which gate failed. Even rejected messages are anchored in MMR.

### P4: Determinism
Given identical inputs (same token, intent, tool, message, contract), the pipeline produces identical decisions. Content inspection uses deterministic regex matching. Contract validation is pure and stateless.

### P5: Least Privilege
Agents can only perform actions explicitly listed in their safety contract. The default is deny — any intent, tool, or parameter shape not in the contract is rejected.

### P6: Delegation Non-Escalation
An agent cannot delegate tasks it is not itself authorized to perform. The orchestration guard checks the delegator's own contract before allowing delegation.

## OWASP Mapping

| Gate | OWASP Categories |
|------|-----------------|
| G1 (Identity) | ASI03 Agent Impersonation, ASI10 Uncontrolled Proliferation |
| G2 (Content) | ASI01 Prompt Injection, ASI06 Sensitive Data Exposure |
| G3 (Contract) | ASI01 Prompt Injection, ASI02 Excessive Agency, ASI06 Data Exposure |
| G4 (Authority) | ASI02 Excessive Agency, ASI09 Insufficient Access Controls |
| G5 (Orchestration) | ASI05 Improper Multi-Agent Orchestration |
| A (Anchor) | ASI04 Insecure Output Handling, ASI07 Communication Integrity, ASI08 Audit Failures |

**Total: 10/10 OWASP Agentic Top 10 covered.**
