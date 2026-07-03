# GPT Image 2 Generation

GPT Image 2 can be used as an asynchronous task-style image generation interface. Create a task first, then poll the task result.

Some compatible channels may return images directly. If the response contains `task_id` or `id`, use the async polling flow.

## Flow

```text
1. POST create image task
2. Read task_id or id
3. GET task status
4. When status=completed, read url / image_url / output / data
5. Download the image
```

## Create Task

```text
POST https://fullstacklab.xyz/v1/images/generations
```

## Headers

```text
Authorization: Bearer YOUR_API_KEY
Content-Type: application/json
```

## Example

```python
import requests

API_KEY = "YOUR_API_KEY"
BASE_URL = "https://fullstacklab.xyz/v1"

payload = {
    "model": "gpt-image-2",
    "prompt": "A cute orange cat playing with yarn",
    "n": 1,
    "size": "1024x1024"
}

headers = {
    "Authorization": f"Bearer {API_KEY}",
    "Content-Type": "application/json"
}

res = requests.post(f"{BASE_URL}/images/generations", headers=headers, json=payload, timeout=120)
res.raise_for_status()
result = res.json()

task_id = result.get("task_id") or result.get("id")
print("Task ID:", task_id)
```

## Poll Result

```python
import time
import requests

def wait_image_result(task_id, interval=3, timeout=420):
    url = f"https://fullstacklab.xyz/v1/images/generations/{task_id}"
    headers = {"Authorization": "Bearer YOUR_API_KEY"}
    start = time.time()

    while True:
        res = requests.get(url, headers=headers, timeout=60)
        res.raise_for_status()
        data = res.json()

        status = str(data.get("status", "")).lower()
        if status == "completed":
            return data.get("url") or data.get("image_url") or data.get("output") or data.get("data")

        if status == "failed":
            raise Exception(data.get("error") or data.get("fail_reason") or data)

        if time.time() - start > timeout:
            raise TimeoutError("Image task timed out")

        time.sleep(interval)
```
