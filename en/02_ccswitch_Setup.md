# ccswitch Setup

ccswitch is used to import Fullstack Lab configuration into local AI tools such as Claude Code and Codex.

## What to Check During Import

When importing configuration, check:

- Provider name
- API endpoint
- Masked API Key

The Base URL should be:

```text
https://fullstacklab.xyz
```

## Manual Custom Configuration

If automatic import does not work, add a custom configuration manually:

1. Open ccswitch.
2. In the provider list, select the `Claude Desktop Official` icon first.
3. Click the `+` button on the far left.
4. Choose `Custom Configuration`.
5. Fill in the following fields:

```text
Request URL / Base URL: https://fullstacklab.xyz
API Key: your real API Key created in the Fullstack Lab dashboard
```

6. Check that the URL has no extra spaces.
7. Check that the API Key is copied completely.
8. Click `Add` or `Save`.
9. Return to the provider list and make sure the new custom configuration is enabled.

After importing or saving the configuration, fully close Claude Code, Codex, and related terminal windows. Then open them again and send a test message:

```text
Please reply: configuration is active.
```

## Common Issues

| Issue | Solution |
| --- | --- |
| Invalid token | Re-copy the API Key from the dashboard |
| Connection failed | Check the Base URL and network |
| Tool still uses old config | Fully close and reopen the tool and terminal |
