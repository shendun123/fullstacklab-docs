# Gemini GenerateContent

Use this endpoint for Gemini-compatible REST calls.

## Endpoint

```text
POST https://fullstacklab.xyz/v1beta/models/{model}:generateContent
```

## Example

```bash
curl "https://fullstacklab.xyz/v1beta/models/gemini-2.5-pro:generateContent" \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -d '{
    "contents": [
      {
        "parts": [
          {"text": "Hello, please reply with one test sentence."}
        ]
      }
    ]
  }'
```

If an SDK does not support custom base URLs well, use REST requests directly.
