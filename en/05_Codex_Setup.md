# Codex Setup

This section explains how to configure Codex on Windows using PowerShell.

## Step 1: Open PowerShell

Open the Windows Start menu, search for `PowerShell`, and open it. Administrator mode is usually not required.

## Step 2: Create Codex Configuration Files

Run these commands one by one:

```powershell
New-Item -ItemType Directory -Force "$env:USERPROFILE\.codex" | Out-Null
New-Item -ItemType File -Force "$env:USERPROFILE\.codex\auth.json" | Out-Null
New-Item -ItemType File -Force "$env:USERPROFILE\.codex\config.toml" | Out-Null
```

## Step 3: Write Codex Configuration

Copy and run:

```powershell
@'
model_provider = "newapi_channel_conn"
model = "gpt-5.5"
model_reasoning_effort = "high"
disable_response_storage = true
preferred_auth_method = "apikey"

[model_providers.newapi_channel_conn]
name = "newapi_channel_conn"
base_url = "https://fullstacklab.xyz/v1"
env_key = "OPENAI_API_KEY"
wire_api = "responses"
stream_idle_timeout_ms = 600000
stream_max_retries = 2
request_max_retries = 2

[windows]
sandbox = "elevated"
'@ | Set-Content -Encoding UTF8 "$env:USERPROFILE\.codex\config.toml"
```

## Step 4: Set the API Key Environment Variable

Replace `your-real-key` with your real API Key from the Fullstack Lab dashboard:

```powershell
[Environment]::SetEnvironmentVariable("OPENAI_API_KEY", "your-real-key", "User")
```

Example format:

```powershell
[Environment]::SetEnvironmentVariable("OPENAI_API_KEY", "sk-xxxxxxxxxxxxxxxx", "User")
```

Do not copy the example key. Use your own real key.

## Step 5: Restart

Fully close PowerShell, Codex, and related terminal windows. Then open them again.

## Step 6: Test

Open Codex and send:

```text
Please reply: configuration is active.
```

If you see `401 Unauthorized`, the API Key is usually wrong, incomplete, or the environment variable has not taken effect.
