# Errors and Troubleshooting

## Common HTTP Status Codes

| Code | Meaning | Suggested Action |
| --- | --- | --- |
| 400 | Bad request | Check required fields and JSON format |
| 401 | Authentication failed | Check API Key |
| 403 | Permission denied | Check model or group permission |
| 429 | Rate limited | Reduce concurrency and retry later |
| 500 | Server error | Retry later or contact support |
| 502/503/504 | Gateway or upstream issue | Retry, switch model, or contact support |
| 522/524 | Connection or timeout issue | Check network, proxy, and timeout settings |

## Task-Based API Issues

| Issue | Solution |
| --- | --- |
| No task_id returned | Print the full response and check whether task creation failed |
| Status stays queued | Increase timeout and keep polling |
| Status is failed | Check `error`, `code`, or `fail_reason` |
| Completed but no URL | Check `url`, `image_url`, `video_url`, `output`, and `data` fields |

## Security

Never publish real API Keys. Use `YOUR_API_KEY` in documentation and examples.
