# Technical Architecture — PYTHON_API_CLIENT

**Upstream:** [https://github.com/recombee/python-api-client](https://github.com/recombee/python-api-client)
**License:** MIT
**Category:** ONLINE_RETAIL
**Anticloud Integration:** PAX L5 Narrow L2 General 27B + AIOSS + Offline-First

## Upstream Description

Recommendation engine for retail

## Anticloud Architectural Changes

1. PAX L5 Narrow L2 General 27B local product recommendation and description generation
2. AIOSS append-only order and inventory audit chain
3. AES-256 encryption for all customer PII and payment tokenization
4. Single-binary e-commerce platform — no cloud hosting required
5. Zero-cloud: all search, recommendation, and analytics run locally
6. GPU/CPU equalizer: AI recommendations on CPU for small catalogs, GPU for large
7. Zero-telemetry: removes all third-party analytics scripts
8. Open cart export: standard CSV/JSON, no vendor lock-in

## Integration Points

- **PAX Inference Socket:** Local HTTP endpoint at `127.0.0.1:11434/v1/chat` — same OpenAI-compatible API, zero cloud
- **AIOSS Hook:** Every write operation calls `aioss_append(event, payload)` before commit
- **Encryption Layer:** All file I/O routed through `anticloud_crypto.encrypt_at_rest()`
- **Single Binary Build:** `pyinstaller anticloud_python_api_client.spec` or `go build -o python_api_client`

## Deployment Modes

| Mode | Hardware | Notes |
| --- | --- | --- |
| Edge CPU | Raspberry Pi 4 / Intel NUC | Full feature set, PAX on CPU |
| Desktop GPU | RTX 3060 / A10 | PAX GPU inference, <1s latency |
| Server | 2× A100 | Full batch throughput |
| Air-gapped | Any x86/ARM | Zero network dependency |