# PSBT Consolidation Failure Analysis

This repository contains technical notes and essays exploring Bitcoin PSBT consolidation risks, operational failure modes, and governance implications at scale.

These documents are intended for Bitcoin infrastructure teams, multisig operators, treasury custodians, auditors, and sovereign funds managing fragmented UTXOs in high-stakes environments.

## Essays & Notes

- [PSBT Consolidation: A Failure-Oriented Analysis of Real-World Bitcoin Operations](./psbt-consolidation-failure-analysis.md)  
  A failure-oriented analysis of real-world PSBT-based consolidation operations, enumerating non-cryptographic failure modes (interface authority creep, cross-signer desynchronization, custody diffusion, audit breakdown) and defining the non-negotiable invariants required for safe execution at scale.

- [Governance Risk in Bitcoin Consolidation: The Need for Testable Frameworks](./governance-risk-in-bitcoin-consolidation.md)  
  Examines the transition of Bitcoin UTXO consolidation from a niche technical concern into a systemic governance risk as adoption and institutional involvement scale; highlights operational, privacy, and financial exposures from ad-hoc processes and calls for deterministic, auditable, testable frameworks to mitigate them.

- [Enforcing Consolidation Invariants Under Volatility: Deterministic Safeguards for Bitcoin Treasury and Multisig Operations](./enforcing-consolidation-invariants-under-volatility.md)  
  Provides practical mechanisms to enforce the required invariants for safe PSBT consolidation under volatile fee regimes, liquidity pressure, and treasury/multisig coordination challenges; extends prior failure analysis to real-world stress conditions and demonstrates how neutral, deterministic tooling makes unsafe states unrepresentable in high-stakes environments.

- [Runtime Governance for Bitcoin Treasury Operations: Making Unsafe States Unrepresentable in PSBT Workflows](./runtime-governance-psbt-invariants.md)  
  Argues for migrating Bitcoin treasury governance from procedural checklists to execution-layer constraints embedded in PSBT construction and validation tooling. Details how to structurally prevent unsafe states (liquidity over-exposure, fee spikes, fragmentation, coordination failures) via deterministic invariants, with concrete specification examples for infrastructure enforcement.

## License

All documents in this repository are licensed under the MIT License unless otherwise noted.
