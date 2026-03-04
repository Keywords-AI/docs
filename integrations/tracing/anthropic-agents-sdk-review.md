# Claude Agent SDK Docs — Review Notes

Docs page: `integrations/tracing/anthropic-agents-sdk.mdx`

## Source of truth

- **Python SDK**: `respan/python-sdks/respan-exporter-anthropic-agents`
- **TypeScript SDK**: `@respan/exporter-anthropic-agents` (npm)
- **Python examples**: `respan-example-projects/python/tracing/anthropic-agents-sdk/`
- **TypeScript examples**: `respan-example-projects/typescript/tracing/anthropic-agents-sdk/`
- **PyPI**: https://pypi.org/project/respan-exporter-anthropic-agents/

## Items to verify

### Setup section

- [ ] Python setup uses `exporter.query()` — matches `_sdk_runtime.py` helper
- [ ] TypeScript setup uses `query()` from SDK + `exporter.trackMessage()` — matches example pattern
- [ ] TypeScript does NOT use `exporter.query()` in examples (uses lower-level `query` + `trackMessage`)
- [ ] Verify `exporter.query()` works in TypeScript as async generator (if so, docs could simplify)

### Configuration table

- [ ] Python: `base_delay_seconds` (default: 1.0) and `max_delay_seconds` (default: 30.0) exist but not documented — decide if worth adding
- [ ] TypeScript: `baseDelaySeconds` and `maxDelaySeconds` exist but not documented
- [ ] TypeScript: no `baseUrl` param — endpoint is constructed from `RESPAN_BASE_URL` env var manually in examples

### TypeScript patterns

The TS examples construct the endpoint manually:
```typescript
endpoint: BASE_URL ? `${BASE_URL.replace(/\/+$/, "")}/v1/traces/ingest` : undefined
```
This differs from Python where `base_url` is a constructor param. The TS SDK only exposes `endpoint` directly.

### Examples section

- [ ] "Wrapped query" example uses Python `_sdk_runtime.query_for_result` helper — docs shows raw `exporter.query()` loop instead (simpler, equivalent)
- [ ] "Tool use" example matches `tools/tool_use_test.py`
- [ ] "Streaming" example matches `streaming/stream_messages_test.py`
- [ ] "Attributes" — auto-captured, verify field names match SDK source (`prompt_tokens` vs `input_tokens`)
- [ ] "Traced events" — verify 5 hook events still correct

### Cross-check with gateway page

- Gateway page (`gateway/anthropic-agents-sdk.mdx`) uses same SDK + `ClaudeAgentOptions.env` for routing
- Gateway examples in `python/gateway/anthropic-agents/` are separate
