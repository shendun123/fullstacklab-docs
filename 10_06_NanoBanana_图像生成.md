# Nano Banana 图像生成

Nano Banana 使用任务式接口：先创建任务，再根据任务 ID 轮询结果。支持文生图和图生图。

## 创建任务

```text
POST https://fullstacklab.xyz/v1/videos
```

虽然路径是 `/videos`，但该任务可用于 Nano Banana 图像生成，具体以平台后台模型能力为准。

## 请求头

```text
Authorization: Bearer YOUR_API_KEY
Content-Type: application/json
```

## 支持模型

| 模型 | 说明 |
| --- | --- |
| nano_banana_2 | 标准版，速度快，适合普通生图测试 |
| nano_banana_pro-1K | 1K 分辨率，适合轻量高清图 |
| nano_banana_pro-2K | 2K 高清细节，适合电商图、海报图 |
| nano_banana_pro-4K | 4K 超高清细节，适合商业级海报和精修图 |

## 支持比例

| 比例 | 适合场景 |
| --- | --- |
| auto | 自动构图 |
| 1:1 | 头像、电商主图、社媒配图 |
| 9:16 | 短视频封面、竖屏图 |
| 16:9 | 官网 Banner、横版广告图 |

## 文生图请求示例

```python
import requests

API_KEY = "YOUR_API_KEY"
BASE_URL = "https://fullstacklab.xyz/v1/videos"

payload = {
    "model": "nano_banana_pro-4K",
    "prompt": "一个可爱的中国女孩，真实摄影风格，自然光线，干净背景，4K超清细节，商业摄影质感",
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
print("任务 ID:", task_id)
```

## 图生图请求示例

```python
payload = {
    "model": "nano_banana_pro-4K",
    "prompt": "把这张图优化成高级商业海报风格，增强光影质感，画面更精致",
    "aspect_ratio": "9:16",
    "images": ["https://example.com/test.jpg"]
}
```

图生图前，参考图 URL 必须是公网可访问的图片直链，并且响应 `Content-Type` 应该是 `image/*`。

## 轮询任务

```python
import time
import requests

def wait_result(task_id):
    url = f"https://fullstacklab.xyz/v1/videos/{task_id}"
    headers = {"Authorization": "Bearer YOUR_API_KEY"}

    while True:
        res = requests.get(url, headers=headers, timeout=60)
        res.raise_for_status()
        data = res.json()

        status = data.get("status")
        progress = data.get("progress", 0)
        print("当前状态:", status, "进度:", progress)

        if status == "completed":
            return data.get("url") or data.get("video_url")

        if status == "failed":
            raise Exception(data.get("error") or data)

        time.sleep(3)
```

## 状态说明

| 状态 | 说明 |
| --- | --- |
| queued | 排队中 |
| processing | 处理中 |
| in_progress | 处理中，兼容状态 |
| completed | 已完成 |
| failed | 失败 |

## 使用建议

- 先跑单张测试，确认模型、比例、Key、网络都正常。
- 全量测试会产生多次计费，例如 4 个模型 × 4 个比例 = 16 次任务。
- 下载图片时建议按模型、比例、时间自动命名，方便归档。
