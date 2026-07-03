# Nano Banana Image Generation

Nano Banana is an asynchronous task-style image generation interface. It supports text-to-image and image-to-image generation.

## Flow

```text
1. POST create task
2. Read task_id or id
3. GET task status
4. When status=completed, read url / image_url / video_url / output
5. Download the image
```

## Create Task

```text
POST https://fullstacklab.xyz/v1/videos
```

Although the path is `/videos`, the task can be used for Nano Banana image generation depending on platform model capability.

## Example

```python
import requests

API_KEY = "YOUR_API_KEY"
BASE_URL = "https://fullstacklab.xyz/v1/videos"

payload = {
    "model": "nano_banana_pro-4K",
    "prompt": "A realistic portrait, natural light, clean background, commercial photography",
    "aspect_ratio": "9:16"
}

headers = {
    "Authorization": f"Bearer {API_KEY}",
    "Content-Type": "application/json"
}

res = requests.post(BASE_URL, headers=headers, json=payload, timeout=60)
res.raise_for_status()
task = res.json()

task_id = task.get("task_id") or task.get("id")
print("Task ID:", task_id)
```

## Poll Result

```python
import time
import requests

def wait_result(task_id, interval=3, timeout=420):
    url = f"https://fullstacklab.xyz/v1/videos/{task_id}"
    headers = {"Authorization": "Bearer YOUR_API_KEY"}
    start = time.time()

    while True:
        res = requests.get(url, headers=headers, timeout=60)
        res.raise_for_status()
        data = res.json()

        status = str(data.get("status", "")).lower()
        if status == "completed":
            image_url = data.get("url") or data.get("video_url") or data.get("image_url") or data.get("output")
            if not image_url:
                raise Exception(f"Task completed but no image URL was returned: {data}")
            return image_url, data

        if status == "failed":
            raise Exception(data.get("error") or data.get("fail_reason") or data)

        if time.time() - start > timeout:
            raise TimeoutError("Image task timed out")

        time.sleep(interval)
```

## Status Values

| Status | Meaning |
| --- | --- |
| queued | Waiting in queue |
| processing | Processing |
| in_progress | Processing |
| completed | Completed |
| failed | Failed |
