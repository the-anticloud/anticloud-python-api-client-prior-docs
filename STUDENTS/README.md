# Students — PYTHON_API_CLIENT_PRIOR_DOCS

**Project:** PYTHON_API_CLIENT_PRIOR_DOCS  
**Category:** ONLINE_RETAIL  
**Upstream:** https://github.com/recombee/python-api-client  
**Pinned commit:** `70a068d52cd329bdbb478ffa5020bdfd2f0a3a83`  
**Assurance:** 16/16 checks passing  
**Ledger head:** `9cf7797f7d6b05bb33827042fe6928628eb2c4505c895b3d6629f86ad6677677`  
**Date:** October 2026

## What this project gives you

A complete worked example of offline-first packaging with a cryptographic audit
chain: source pinned at `70a068d52cd329bdbb478ffa5020bdfd2f0a3a83`, a 16-check assurance suite, an evidence register
with per-check hashes, and an AIOSS ledger chain ending at `9cf7797f7d6b05bb33827042fe6928628eb2c4505c895b3d6629f86ad6677677`.

## Learn by verifying

```
python tools/run_bench.py --out BENCH.json
```

Then take any row from `ISOLATED_LAB_RESULTS/03_Result_Register.md`, recompute
the SHA3-256 of its evidence file, and confirm it matches. If it does not match,
the record has been altered — that is the whole point of the chain.

## Licence

Apache 2.0 terms apply to study and teaching use.
