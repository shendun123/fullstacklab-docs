# GPT Image 2 图像生成

GPT Image 2 在平台中建议按**异步任务式接口**理解和接入：先创建任务，拿到 `task_id` 或 `id`，再轮询任务结果。

> 说明：有些兼容通道可能会直接返回 `data`、`url` 或 `b64_json`，这种属于同步返回格式；但如果返回的是 `task_id` / `id`，就必须按异步任务方式继续查询结果。

## 一、异步任务式流程

```text
1. POST 创建生图任务
2. 从返回结果中读取 task_id 或 id
3. GET 查询任务状态
4. status=completed 时读取 url / image_url / output / data
5. 下载图片
```

## 二、创建任务

```text
POST https://fullstacklab.xyz/v1/images/generations
```

## 请求头

```text
Authorization: Bearer YOUR_API_KEY
Content-Type: application/json
```

## 请求参数

| 参数 | 类型 | 必填 | 示例 | 说明 |
| --- | --- | --- | --- | --- |
| model | string | 是 | gpt-image-2 | 图像模型名称 |
| prompt | string | 是 | 一只可爱的橘猫 | 图片提示词 |
| n | integer | 否 | 1 | 生成张数 |
| size | string | 否 | 1024x1024 | 图片尺寸 |

## 三、创建任务示例

```python
import requests

API_KEY = "YOUR_API_KEY"
BASE_URL = "https://fullstacklab.xyz/v1"

headers = {
    "Authorization": f"Bearer {API_KEY}",
    "Content-Type": "application/json"
}

payload = {
    "model": "gpt-image-2",
    "prompt": "A cute orange cat playing with yarn, studio ghibli style",
    "n": 1,
    "size": "1024x1024"
}

res = requests.post(
    f"{BASE_URL}/images/generations",
    headers=headers,
    json=payload,
    timeout=120
)

print("状态码:", res.status_code)
print("返回内容:", res.text)

res.raise_for_status()
result = res.json()

task_id = result.get("task_id") or result.get("id")
print("任务 ID:", task_id)

if not task_id:
    print("当前通道可能是同步返回，请检查 data / url / b64_json 字段")
```

## 四、轮询任务结果

如果创建任务返回了 `task_id` 或 `id`，继续轮询：

```python
import time
import requests

API_KEY = "YOUR_API_KEY"
BASE_URL = "https://fullstacklab.xyz/v1"

def wait_image_result(task_id, interval=3, timeout=420):
    url = f"{BASE_URL}/images/generations/{task_id}"
    headers = {
        "Authorization": f"Bearer {API_KEY}"
    }

    start = time.time()

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
            return (
                data.get("url")
                or data.get("image_url")
                or data.get("output")
                or data.get("data")
            )

        if status == "failed":
            raise Exception(data.get("error") or data.get("fail_reason") or data)

        if time.time() - start > timeout:
            raise TimeoutError(f"任务超时，超过 {timeout} 秒仍未完成")

        time.sleep(interval)
```

## 五、保存图片

轮询完成后，可能返回图片 URL，也可能返回包含图片信息的对象。可以按下面方式兼容：

```python
import base64
import time
from pathlib import Path
import requests

def save_image(result, save_dir="./generated_images"):
    save_dir = Path(save_dir)
    save_dir.mkdir(parents=True, exist_ok=True)

    # 情况 1：直接是图片 URL
    if isinstance(result, str) and result.startswith("http"):
        img = requests.get(result, timeout=120)
        img.raise_for_status()
        path = save_dir / f"image_{int(time.time())}.png"
        path.write_bytes(img.content)
        return path

    # 情况 2：返回 data 数组
    if isinstance(result, dict):
        items = result.get("data", [])
        for index, item in enumerate(items, start=1):
            if "url" in item:
                img = requests.get(item["url"], timeout=120)
                img.raise_for_status()
                path = save_dir / f"image_{index}_{int(time.time())}.png"
                path.write_bytes(img.content)

            elif "b64_json" in item:
                data = base64.b64decode(item["b64_json"])
                path = save_dir / f"image_{index}_{int(time.time())}.png"
                path.write_bytes(data)
```

## 六、同步返回兼容说明

如果你的请求没有返回 `task_id` 或 `id`，而是直接返回：

```text
data
url
b64_json
```

说明当前通道是同步返回，可以直接保存图片，不需要轮询。

## 七、排查建议

| 现象 | 可能原因 | 处理方式 |
| --- | --- | --- |
| 返回 `task_id` 但没有图片 | 异步任务还没完成 | 继续轮询任务结果 |
| 一直 `queued` / `processing` | 上游排队或处理慢 | 增加轮询总超时时间 |
| `failed` | 提示词、模型、权限或上游失败 | 打印 `error` / `fail_reason` |
| 请求超时 | 生图耗时较长 | 把 timeout 调到 120 秒或 300 秒 |
| 返回没有 `url` | 字段格式不同 | 同时兼容 `url`、`image_url`、`output`、`data`、`b64_json` |

## 八、安全提醒

示例代码中的：

```text
YOUR_API_KEY
```

需要替换成你自己在全栈实验室后台创建的真实 Key。不要把真实 Key 写进 GitHub、公开文档、截图或群聊。
