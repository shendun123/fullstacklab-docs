# Nano Banana 图像生成

Nano Banana 是**异步任务式图像生成接口**：先创建任务，再根据任务 ID 轮询结果。支持文生图和图生图。

## 一、异步任务式流程

```text
1. POST 创建生图任务
2. 从返回中读取 task_id 或 id
3. GET 查询任务状态
4. status=completed 时读取 url / video_url / image_url / output
5. 下载图片
```

## 二、创建任务

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

## 三、文生图创建任务示例

### Shell

```bash
curl -X POST "https://fullstacklab.xyz/v1/videos" \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "nano_banana_pro-4K",
    "prompt": "一个可爱的中国女孩，真实摄影风格，自然光线，干净背景，4K超清细节，商业摄影质感",
    "aspect_ratio": "9:16"
  }'
```

### Python

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

print("状态码:", res.status_code)
print("返回内容:", res.text)

res.raise_for_status()
task = res.json()

task_id = task.get("task_id") or task.get("id")
print("任务 ID:", task_id)

if not task_id:
    raise Exception(f"返回结果中没有 task_id/id: {task}")
```

### Java

```java
import java.net.URI;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;

public class NanoBananaExample {
    public static void main(String[] args) throws Exception {
        String apiKey = "YOUR_API_KEY";
        String body = """
            {
              "model": "nano_banana_pro-4K",
              "prompt": "一个可爱的中国女孩，真实摄影风格，自然光线，干净背景，4K超清细节，商业摄影质感",
              "aspect_ratio": "9:16"
            }
            """;

        HttpRequest request = HttpRequest.newBuilder()
            .uri(URI.create("https://fullstacklab.xyz/v1/videos"))
            .header("Authorization", "Bearer " + apiKey)
            .header("Content-Type", "application/json")
            .POST(HttpRequest.BodyPublishers.ofString(body))
            .build();

        HttpResponse<String> response = HttpClient.newHttpClient()
            .send(request, HttpResponse.BodyHandlers.ofString());

        System.out.println(response.body());
    }
}
```

## 四、图生图创建任务示例

```python
payload = {
    "model": "nano_banana_pro-4K",
    "prompt": "把这张图优化成高级商业海报风格，增强光影质感，画面更精致",
    "aspect_ratio": "9:16",
    "images": ["https://example.com/test.jpg"]
}
```

图生图前，参考图 URL 必须是公网可访问的图片直链，并且响应 `Content-Type` 应该是 `image/*`。

## 五、轮询任务结果

```python
import time
import requests

API_KEY = "YOUR_API_KEY"

def wait_result(task_id, interval=3, timeout=420):
    url = f"https://fullstacklab.xyz/v1/videos/{task_id}"
    headers = {
        "Authorization": f"Bearer {API_KEY}"
    }

    start_time = time.time()

    while True:
        res = requests.get(url, headers=headers, timeout=60)

        print("查询状态码:", res.status_code)
        print("查询返回:", res.text)

        res.raise_for_status()
        data = res.json()

        status = str(data.get("status", "")).lower()
        progress = data.get("progress", 0)

        print("当前状态:", status, "进度:", progress)

        if status == "completed":
            image_url = (
                data.get("url")
                or data.get("video_url")
                or data.get("image_url")
                or data.get("output")
            )

            if not image_url:
                raise Exception(f"任务完成但没有返回图片地址: {data}")

            return image_url, data

        if status == "failed":
            error = data.get("error", {})
            message = error.get("message") if isinstance(error, dict) else error
            raise Exception(message or data.get("fail_reason") or data)

        if status not in ["queued", "processing", "in_progress"]:
            print("未知状态，继续轮询:", status)

        if time.time() - start_time > timeout:
            raise TimeoutError(f"任务超时，超过 {timeout} 秒仍未完成")

        time.sleep(interval)
```

## 六、下载图片

```python
from pathlib import Path
import time
import requests

def download_image(image_url, save_dir="./banana_images"):
    save_dir = Path(save_dir)
    save_dir.mkdir(parents=True, exist_ok=True)

    res = requests.get(image_url, timeout=120)
    res.raise_for_status()

    save_path = save_dir / f"banana_{int(time.time())}.png"
    save_path.write_bytes(res.content)

    print("下载完成:", save_path)
    return save_path
```

## 七、状态说明

| 状态 | 说明 |
| --- | --- |
| queued | 排队中 |
| processing | 处理中 |
| in_progress | 处理中，兼容状态 |
| completed | 已完成 |
| failed | 失败 |

## 八、完整调用顺序

```python
task_id = create_task(...)
image_url, raw_data = wait_result(task_id)
download_image(image_url)
```

## 九、使用建议

- Nano Banana 是异步任务式接口，不是请求后立即返回图片。
- 先跑单张测试，确认模型、比例、Key、网络都正常。
- 全量测试会产生多次计费，例如 4 个模型 × 4 个比例 = 16 次任务。
- 轮询间隔建议 3 秒左右，任务总超时建议 420 秒左右。
- 下载图片时建议按模型、比例、时间自动命名，方便归档。
- 图生图前先检查参考图 URL 是否可公网访问。
