# Ethics — PYTHON_API_CLIENT_PRIOR_DOCS

**Project:** PYTHON_API_CLIENT_PRIOR_DOCS  
**Category:** ONLINE_RETAIL  
**Upstream:** https://github.com/recombee/python-api-client  
**Pinned commit:** `70a068d52cd329bdbb478ffa5020bdfd2f0a3a83`  
**Assurance:** 16/16 checks passing  
**Ledger head:** `9cf7797f7d6b05bb33827042fe6928628eb2c4505c895b3d6629f86ad6677677`  
**Date:** October 2026

## Position

PYTHON_API_CLIENT_PRIOR_DOCS is packaged for offline deployment with a verifiable audit trail. The
ethical questions this raises are answered by making the system's behaviour
checkable rather than by policy statements.

## The four commitments

1. **No hidden egress.** The deployment has no external API dependency; this is
   testable by running it with the network disconnected.
2. **Attributable output.** Every artifact is recorded in a hash chain, so what
   the system produced can be reconstructed.
3. **Operator control.** The institution owns the hardware and the keys.
4. **Refusal to overclaim.** Where a certification is not held, the project says
   so rather than implying it.

## Dual use

This project is packaged for civilian and public-sector deployment. Where an
upstream has dual-use characteristics, the licence gate and the reference-only
marking in `BENCH.json` record that.
