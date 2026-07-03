# Anthropic Messages API

Use this endpoint for Anthropic-style message requests.

## Endpoint

```text
POST https://fullstacklab.xyz/v1/messages
```

## Headers

```text
Authorization: Bearer YOUR_API_KEY
Content-Type: application/json
anthropic-version: 2023-06-01
```

## Example

```bash
curl "https://fullstacklab.xyz/v1/messages" \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "anthropic-version: 2023-06-01" \
  -d '{
    "model": "claude-sonnet-4-20250514",
    "max_tokens": 1024,
    "messages": [
      {"role": "user", "content": "Hello"}
    ]
  }'
```
