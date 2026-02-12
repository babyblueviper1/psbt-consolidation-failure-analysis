# PSBT Consolidation  
## A Failure-Oriented Analysis of Real-World Bitcoin Operations

**Version:** 1.0  
**Status:** Public Technical Note  
**Audience:** Bitcoin infrastructure teams, multisig operators, custodians, auditors

---

## Abstract

Partially Signed Bitcoin Transactions (PSBTs) are widely adopted to coordinate multisignature Bitcoin spending while minimizing private-key exposure. However, **UTXO consolidation performed via PSBT introduces a distinct and under-documented class of operational failures**.

These failures do not originate in cryptography.  
They emerge from **process design, interface authority, and coordination breakdowns** under real-world conditions.

This document enumerates those failure modes and specifies **non-negotiable invariants** required for safe PSBT-based consolidation at scale.

---

## Scope and Definitions

**PSBT Consolidation** refers to any transaction whose primary purpose is to:

- Reduce UTXO count  
- Reorganize wallet structure  
- Migrate funds between policy states  
- Prepare funds for future spending efficiency  

This explicitly excludes:

- Routine payments  
- Batched withdrawals  
- Policy-driven spending  

Consolidation is treated here as a **high-risk, non-reversible operation**.

---

## Incorrect Safety Assumption

PSBTs are often assumed to be “safe by default” because:

- Signing is separated from construction  
- Keys remain offline  
- Multiple approvals are required  

This assumption fails during consolidation because:

- Transaction scope is large  
- Errors propagate across signers  
- Review complexity exceeds human reliability  
- Mempool conditions introduce time pressure  

At this point, safety becomes **systemic**, not cryptographic.

---

## Failure Mode: Interface-Derived Authority

Many consolidation failures originate from interface behavior rather than operator intent.

Observed patterns include:

- Dynamic UTXO selection abstracted away from signers  
- Automatic change output generation  
- Fee logic that mutates transaction structure after review  
- Implicit input or output reordering  

These behaviors silently **transfer authority from operators to software**.

Once this occurs, the PSBT model no longer provides meaningful protection.

---

## Failure Mode: Cross-Signer Desynchronization

In distributed signing environments:

- Signers review transactions at different times  
- Using different tools  
- Under different assumptions  

If a PSBT can be modified after partial signing, then:

- Signatures do not imply shared understanding  
- Quorum becomes symbolic  
- Consensus is illusory  

This failure mode is rarely detected before broadcast.

---

## Failure Mode: Consolidation as a One-Way Door

Unlike routine transactions, consolidation errors:

- Cannot be easily reversed  
- Affect future privacy and spendability  
- Increase long-term operational cost  
- Amplify blast radius  

These costs compound invisibly and persist indefinitely.

---

## Failure Mode: Custody Creep

Consolidation frequently becomes centralized because:

- Coordination overhead is high  
- Responsibility diffuses  
- Convenience is prioritized  

This leads to:

- A single party assembling transactions  
- Others acting as passive signers  
- De facto custody without explicit mandate  

This is a governance failure, not a technical one.

---

## Failure Mode: Audit Breakdown

Post-incident analysis often fails due to:

- Missing construction logs  
- Lack of UTXO selection rationale  
- Discarded intermediate PSBT states  
- Absence of immutable provenance records  

As a result, organizations cannot reliably answer:

> Who decided this transaction’s structure?

This undermines accountability and compliance.

---

## Required Invariants for Safe Consolidation

Any PSBT consolidation process must enforce the following invariants **by construction**:

1. **Deterministic Scope**  
   Inputs and outputs are fixed before signing begins.

2. **Signer Symmetry**  
   All signers review the same transaction representation.

3. **Interface Non-Authority**  
   No interface may alter structure after review.

4. **Single-Purpose Execution**  
   Consolidation is isolated from spending logic.

5. **Non-Custodial Assembly**  
   No actor gains unilateral transaction authority.

6. **Immutable Logging**  
   Every step is auditable and preserved.

Violation of any invariant renders the process unsafe.

---

## Why These Failures Persist

These failures persist because:

- They are low-frequency but high-impact  
- They evade happy-path testing  
- UX incentives favor flexibility  
- Accountability diffuses under scale  

They are typically recognized only after exposure has already occurred.

---

## Conclusion: Consolidation Is a Constraint Problem

PSBT consolidation safety does not emerge from:

- Better user interfaces  
- Additional features  
- Operator training  

It emerges from **intentional limitation**.

Systems that permit discretionary scope, silent mutation, or post-hoc rationalization will eventually fail.

---

## Appendix A: Reference Implementation

**Ωmega Pruner** is a reference implementation designed to enforce the invariants described in this document.

It is:

- Deterministic by design  
- PSBT-only  
- Single-purpose (consolidation only)  
- Non-custodial  
- Invariant-enforcing rather than feature-driven  

Ωmega Pruner does not attempt to optimize for convenience.  
It exists to make unsafe states **unrepresentable**.

The implementation is publicly available and may be evaluated as:

- A standalone consolidation tool  
- An embedded engine within existing systems  
- A procedural reference for internal implementations  

---

## Intended Use

This document is intended to:

- Inform internal technical reviews  
- Support audit and compliance discussions  
- Surface systemic operational risk  
- Frame consolidation as a governance issue  

It is not a sales document.

---

## License

MIT License
