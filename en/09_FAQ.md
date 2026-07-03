# FAQ

## 1. Why do I get 401 Unauthorized?

Usually caused by:

- Wrong API Key
- Incomplete API Key copy
- Expired or deleted key
- Environment variable not refreshed
- Placeholder key still used in the code

Fix: re-copy your real key from the dashboard and restart the tool or terminal.

## 2. Why does the tool still use old settings?

The tool or terminal may still be using cached environment variables.

Fix:

1. Close the tool.
2. Close all related terminals.
3. Reopen the terminal.
4. Reopen the tool.

## 3. Why does image or video generation take a long time?

Image and video generation can be slower than text generation. Use longer request timeouts and task polling.

## 4. Why does task status stay queued or processing?

The task may be waiting for upstream capacity. Keep polling with a reasonable interval, such as 3 to 5 seconds.

## 5. Why is there no result URL after completion?

Different channels may use different fields. Check:

```text
url
image_url
video_url
output
data
```

## 6. Can I share my API Key?

No. Never share your full API Key in screenshots, public documents, GitHub, or chat groups.
