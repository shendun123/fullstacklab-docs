# OpenAI Responses API

Use this endpoint for Responses API compatible clients such as Codex.

## Endpoint

```text
POST https://fullstacklab.xyz/v1/responses
```

## Example

```bash
curl -X POST "https://fullstacklab.xyz/v1/responses" \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "gpt-5.5",
    "input": "Hello, please reply with one test sentence."
  }'
```

## Codex Note

For Codex, the configuration often uses:

```toml
wire_api = "responses"
```
