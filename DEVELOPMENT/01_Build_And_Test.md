# Build and Test

**Project:** `PYTHON_API_CLIENT`
**Upstream:** https://github.com/recombee/python-api-client
**License:** MIT

## Quick Start

```bash
git clone https://github.com/recombee/python-api-client
cd python-api-client
pip install -r requirements-anticloud.txt
python anticloud_main.py --offline --pax-local
```

## Anticloud Improvements Applied

1. PAX L5 Narrow L2 General 27B local product recommendation and description generation
2. AIOSS append-only order and inventory audit chain
3. AES-256 encryption for all customer PII and payment tokenization
4. Single-binary e-commerce platform — no cloud hosting required
5. Zero-cloud: all search, recommendation, and analytics run locally
6. GPU/CPU equalizer: AI recommendations on CPU for small catalogs, GPU for large
7. Zero-telemetry: removes all third-party analytics scripts
8. Open cart export: standard CSV/JSON, no vendor lock-in

## Benchmark Targets

| Metric | Target |
| --- | --- |
| Latency | Primary inference task: <5s on CPU, <1s on GPU |
| Throughput | Batch processing: >100 items/hour on single CPU server |
| Memory | <8GB RAM for standard deployment |
| Accuracy | Task-specific accuracy within 5% of cloud-API baseline |

## Build Status

Not yet measured. Run verified build and record actual figures above.
