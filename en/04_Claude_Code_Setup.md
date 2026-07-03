# Claude Code Setup

Claude Code is usually installed through Node.js and npm.

## Check Node.js

```bash
node -v
npm -v
```

If Node.js is not installed, install it first.

## Install Claude Code

```bash
npm install -g @anthropic-ai/claude-code
```

Check installation:

```bash
claude --version
```

## Configuration Notes

Depending on your setup method, Claude Code may use environment variables or a configuration file.

Common fields:

```text
ANTHROPIC_API_KEY
ANTHROPIC_BASE_URL
```

Use the API Key created in the Fullstack Lab dashboard. Do not use a sample key.

After updating configuration, fully close Claude Code and all related terminal windows. Then reopen Claude Code and test with a simple prompt.

## Test Prompt

```text
Please reply: configuration is active.
```
