# Veo Fast Video Generation

Veo Fast uses a task-based workflow. Create a video task first, then poll for the result.

## Create Task

```text
POST https://fullstacklab.xyz/v1/videos
```

## Query Task

```text
GET https://fullstacklab.xyz/v1/videos/{task_id}
```

## Example

```python
import requests

API_KEY = "YOUR_API_KEY"

payload = {
    "model": "veo3.1-fast",
    "prompt": "A cinematic shot of a futuristic city at night",
    "size": "720x1280",
    "seconds": "8"
}

headers = {
    "Authorization": f"Bearer {API_KEY}",
    "Accept": "application/json"
}

res = requests.post("https://fullstacklab.xyz/v1/videos", headers=headers, data=payload, timeout=300)
res.raise_for_status()
task = res.json()

task_id = task.get("id") or task.get("task_id")
print("Task ID:", task_id)
```

Recommended polling interval: 5 seconds. Recommended total timeout: around 600 seconds.
