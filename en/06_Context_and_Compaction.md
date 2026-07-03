# Context and Compaction

The context window is the amount of conversation history, file content, tool output, and other information that a model can process in a single request.

When a conversation becomes long, the tool may compact or summarize earlier messages. This helps the session continue but may lose some details.

## Recommended Practice

- Keep important rules in memory files.
- Keep project commands in a stable document.
- Avoid relying only on old chat history for critical details.
- Start a new session when the previous one becomes too long.

## Common Setting

For Claude Code, users may adjust compaction behavior with:

```text
CLAUDE_AUTOCOMPACT_PCT_OVERRIDE=78
```

Use this only if you understand what it does.
