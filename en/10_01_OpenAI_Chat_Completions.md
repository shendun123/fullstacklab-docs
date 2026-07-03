# OpenAI Chat Completions

Use this endpoint for standard OpenAI-compatible chat completion requests.

## Endpoint

```text
POST https://fullstacklab.xyz/v1/chat/completions
```

## Headers

```text
Authorization: Bearer YOUR_API_KEY
Content-Type: application/json
```

## Example

```bash
curl "https://fullstacklab.xyz/v1/chat/completions" \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -d '{
    "model": "gpt-5.5",
    "messages": [
      {"role": "user", "content": "Hello, please reply with one test sentence."}
    ]
  }'
```

## Common Parameters

| Parameter | Required | Description |
| --- | --- | --- |
| model | Yes | Model name |
| messages | Yes | Chat messages |
| stream | No | Enable streaming |
| max_tokens | No | Maximum output tokens |
