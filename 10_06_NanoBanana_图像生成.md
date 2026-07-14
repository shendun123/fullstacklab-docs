# Nano Banana 图像生成

Nano Banana 支持两种调用方式：OpenAI Chat Completions 兼容的同步生图，以及 `/v1/videos` 异步任务式生图。同步方式适合聊天客户端直接展示图片，异步方式适合 1K、2K、4K 任务和服务端轮询。

> 以下模型和调用方式已于 2026-07-14 在 `https://fullstacklab.xyz` 实际生成图片成功。同步与异步模型名不同，必须按对应接口原样填写。

## 一、调用方式对照

| 调用方式 | 接口 | 模型名格式 | 返回方式 |
| --- | --- | --- | --- |
| OpenAI 兼容同步生图 | `POST /v1/chat/completions` | 连字符，如 `nano-banana-2` | 等待生成完成，在聊天内容中返回 Markdown 图片 |
| 异步任务式生图 | `POST /v1/videos` | 下划线及分辨率后缀，如 `nano_banana_pro-2K` | 先返回任务 ID，再轮询图片地址 |

不要混用 `nano-banana-2` 与 `nano_banana_2`。前者用于同步 Chat Completions，后者用于异步 `/v1/videos`；接口或模型名搭配错误通常会返回 `404`。

## 二、OpenAI 兼容同步生图

### 接口地址

```text
POST https://fullstacklab.xyz/v1/chat/completions
```

### 实测可用模型

| 模型 | 实测返回 | 说明 |
| --- | --- | --- |
| `nano-banana-2` | Markdown 公网图片 URL | 成功，偶发 `524` 时可有限重试 |
| `nano-banana-pro` | Markdown 公网图片 URL | 成功 |
| `nano-banana-2-pro` | Markdown 公网图片 URL | 成功 |

本轮请求均成功生成 `1024x1024` PNG 图片。该尺寸是本轮测试结果，不代表所有参数组合下只支持这一尺寸。

### Shell 示例

```bash
curl -X POST "https://fullstacklab.xyz/v1/chat/completions" \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "nano-banana-pro",
    "messages": [
      {
        "role": "user",
        "content": "白色背景，一个红色圆形，圆形中央写着 TEST，极简、清晰"
      }
    ],
    "stream": false
  }'
```

### Python 示例

```python
from pathlib import Path

import requests

API_KEY = "YOUR_API_KEY"

response = requests.post(
    "https://fullstacklab.xyz/v1/chat/completions",
    headers={
        "Authorization": f"Bearer {API_KEY}",
        "Content-Type": "application/json",
    },
    json={
        "model": "nano-banana-pro",
        "messages": [
            {
                "role": "user",
                "content": "白色背景，一个红色圆形，圆形中央写着 TEST，极简、清晰",
            }
        ],
        "stream": False,
    },
    timeout=(15, 180),
)
response.raise_for_status()

content = response.json()["choices"][0]["message"]["content"].strip()
prefix = "![image]("
if not content.startswith(prefix) or not content.endswith(")"):
    raise RuntimeError(f"未找到图片结果: {content[:300]}")

image_url = content[len(prefix):-1]
image_response = requests.get(image_url, timeout=120)
image_response.raise_for_status()

output = Path("nano-banana.png")
output.write_bytes(image_response.content)
print("图片 URL:", image_url)
print("图片已保存:", output.resolve())
```

同步请求会持续等待图片生成，客户端超时时间建议至少设置为 180 秒。遇到 `502`、`503` 或 `524` 时，可采用退避策略有限重试 1 至 2 次，避免无间隔重复提交导致重复计费。

## 三、异步任务式流程

```text
1. POST 创建生图任务
2. 从返回中读取 task_id 或 id
3. GET 查询任务状态
4. status=completed 时读取 url / video_url / image_url / output
5. 下载图片
```

## 四、创建异步任务

```text
POST https://fullstacklab.xyz/v1/videos
```

虽然路径是 `/videos`，但该任务可用于 Nano Banana 图像生成，具体以平台后台模型能力为准。

### 请求头

```text
Authorization: Bearer YOUR_API_KEY
Content-Type: application/json
```

### 实测可用模型

| 模型 | 本轮实际图片尺寸 | 说明 |
| --- | --- | --- |
| `nano_banana_2` | `1024x1024` | 标准版，适合普通生图测试 |
| `nano_banana_pro-1K` | `1024x1024` | 1K 分辨率，适合轻量高清图 |
| `nano_banana_pro-2K` | `2048x2048` | 2K 高清细节，适合电商图、海报图 |
| `nano_banana_pro-4K` | `4096x4096` | 4K 超高清细节，适合商业级海报和精修图 |

### 支持比例

| 比例 | 适合场景 |
| --- | --- |
| auto | 自动构图 |
| 1:1 | 头像、电商主图、社媒配图 |
| 9:16 | 短视频封面、竖屏图 |
| 16:9 | 官网 Banner、横版广告图 |

参考图支持 JPEG、PNG、WEBP，最多 5 张，单张建议不超过 10 MB。URL 必须是公网可访问的图片直链；Base64 必须包含完整 Data URL 前缀。Pro 模型的分辨率由模型名后缀决定，不需要额外传 `resolution`；画面比例仍通过 `aspect_ratio` 设置。

## 五、文生图创建任务示例

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

res = requests.post(BASE_URL, headers=headers, json=payload, timeout=(15, 180))

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

## 六、图生图创建任务示例

```python
payload = {
    "model": "nano_banana_pro-4K",
    "prompt": "把这张图优化成高级商业海报风格，增强光影质感，画面更精致",
    "aspect_ratio": "9:16",
    "images": ["https://example.com/test.jpg"]
}
```

图生图前，参考图 URL 必须是公网可访问的图片直链，并且响应 `Content-Type` 应该是 `image/*`。

## 七、轮询任务结果

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

## 八、下载图片

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

## 九、状态说明

| 状态 | 说明 |
| --- | --- |
| queued | 排队中 |
| processing | 处理中 |
| in_progress | 处理中，兼容状态 |
| completed | 已完成 |
| failed | 失败 |

## 十、完整调用顺序

```python
# task_id 来自前面的创建任务响应
image_url, raw_data = wait_result(task_id)
download_image(image_url)
```

## 十一、使用建议

- 同步 Chat Completions 会等待并直接返回 Markdown 图片；异步 `/v1/videos` 会先返回任务 ID。
- 先跑单张测试，确认模型、比例、Key、网络都正常。
- 全量测试会产生多次计费，例如 4 个模型 × 4 个比例 = 16 次任务。
- 轮询间隔建议 3 秒左右，任务总超时建议 420 秒左右。
- 下载图片时建议按模型、比例、时间自动命名，方便归档。
- 图生图前先检查参考图 URL 是否可公网访问。
