# Cherry Studio Setup

Cherry Studio is a desktop client that supports OpenAI-compatible providers.

## Installation

Download Cherry Studio from its official release page and install the version for your operating system.

## Configuration Steps

After installation:

1. Open Cherry Studio.
2. Go to `Settings`.
3. Find one of these entries depending on your version:
   - Model Providers
   - API Settings
   - OpenAI Compatible
   - OpenAI-Compatible Provider
   - Custom API
4. Click `Add`, `Add Provider`, or `Add Model Service`.
5. Select `OpenAI Compatible` or `Custom OpenAI API`.
6. Fill in:

```text
Provider Name: Fullstack Lab
API Address / Base URL: https://fullstacklab.xyz/v1
API Key: your real API Key created in the Fullstack Lab dashboard
Default Model: gpt-5.5
```

7. Save the configuration.
8. Return to the model list and select the Fullstack Lab provider.
9. Create a new chat and send:

```text
Please reply: configuration is active.
```

If the model replies normally, Cherry Studio is connected successfully.

## Troubleshooting

| Issue | Solution |
| --- | --- |
| Connection failed | Check that the Base URL is correct |
| Authentication failed | Check the API Key |
| No model appears | Manually enter a model name that is enabled in your dashboard |
| Menu names differ | Look for API, provider, OpenAI compatible, or custom API options |
