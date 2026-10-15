# NIST SP 800-218 SSDF v1.1

**Project:** PYTHON_API_CLIENT_PRIOR_DOCS · **Category:** ONLINE_RETAIL · **Vendor:** Anticloud FZ LLE

## Model component (PAX L5 Narrow L2 General 27B — quoted with provenance)

| Module | Key Metric | Value |
|--------|-----------|-------|
| CRDT | lww_set_us | 0.51 |
| CRDT | merge_10k_ms | 0.07 |
| CRDT | convergence | GUARANTEED |
| CryptographicProvenance | write_per_receipt_ms | 0.059 |
| CryptographicProvenance | verify_1k_chain_ms | 2.53 |
| ZeroTrustLocalMesh | ca_gen_ms | 367.5 |
| ZeroTrustLocalMesh | cert_issue_ms | 333.8 |
| ZeroTrustLocalMesh | tls_version | 1.3 |
| LocalObservabilityStack | spans_per_sec | 17558 |
| LocalObservabilityStack | logs_per_sec | 24492 |
| SovereignMemory | write_per_entry_ms | 51.94 |
| SovereignMemory | encryption | AES-256-GCM |
| AIOSS_AuditChain | entries_per_sec | 232665 |
| AIOSS_AuditChain | hash_algo | SHA3-256 |

*Run ID loiskleinner/pax-benchmark-runner v3, 2026-10-02, Kaggle T4 x2 (2x 16GB VRAM), Ubuntu 22.04, Python 3.12. These are PAX L5 Narrow L2 General 27B model-level results — shared model component, not this project's verdict.*

## Project-level basis (this project's own measured checks)

| Basis check | Status | Observed |
|---|---|---|
| 05_git_health | PASS | head=1b6ba8ed840bc18a8e75af123592083341a35052 commits=1 clea |
| 07_owasp_top10 | PASS | 9/9 controls evidenced (100.0%) |

## Verdict

CONTROLS EVIDENCED — all basis checks PASS

Verdict derives only from the basis checks above (actual runs, register recorded). Nothing here is copied from another project's assessment.

## Cryptographic integrity

- BENCH.json sha3-256: `dcab1fb25132bd838bff422b7f97e0a327ea5983654d4c36e90345636be1d054`
- Result register sha3-256: `6740518ac81139f95ebade82d57e0249d3b5c4af9445c8ceaf766a49e2f16536`
- Generated: 2026-10-06T18:11:26Z

*Anticloud FZ LLE · AIOSS Ledger Genesis: 8b4a8a4f6312dfbe885de8280716985637c163fd2a4b5590341d56db1cc4e560*

> Self-assessment only. No external audit has been performed. A SOC 2 / NIST independent audit is committed post-funding.
