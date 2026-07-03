# Veo Fast 视频生成

Veo Fast 使用任务式接口：先创建视频任务，再根据任务 ID 轮询视频结果。

## 创建任务

```text
POST https://fullstacklab.xyz/v1/videos
```

## 查询任务

```text
GET https://fullstacklab.xyz/v1/videos/{task_id}
```

## 请求头

```text
Authorization: Bearer YOUR_API_KEY
Accept: application/json
```

## 创建任务参数

| 参数 | 类型 | 示例 | 说明 |
| --- | --- | --- | --- |
| model | string | veo3.1-fast | 视频模型 |
| prompt | string | 一个女孩在雨夜街头奔跑 | 视频描述 |
| size | string | 720x1280 | 视频尺寸 |
| seconds | string / number | 8 | 视频时长 |

## Python 创建任务示例

```python
import requests

API_KEY = "YOUR_API_KEY"
BASE_URL = "https://fullstacklab.xyz"

headers = {
    "Authorization": f"Bearer {API_KEY}",
    "Accept": "application/json"
}

payload = {
    "model": "veo3.1-fast",
    "prompt": "一个电影感镜头，未来城市夜景，霓虹灯，高级质感",
    "size": "720x1280",
    "seconds": "8"
}

res = requests.post(
    f"{BASE_URL}/v1/videos",
    headers=headers,
    data=payload,
    timeout=300
)

print(res.status_code)
print(res.text)

task = res.json()
task_id = task.get("id") or task.get("task_id")
print("任务 ID:", task_id)
```

## 轮询任务示例

```python
import time
import requests

def query_task(task_id):
    url = f"https://fullstacklab.xyz/v1/videos/{task_id}"
    headers = {
        "Authorization": f"Bearer {API_KEY}",
        "Accept": "application/json"
    }
    res = requests.get(url, headers=headers, timeout=60)
    res.raise_for_status()
    return res.json()

def wait_for_result(task_id, interval=5, timeout=600):
    start = time.time()

    while time.time() - start < timeout:
        data = query_task(task_id)
        status = str(data.get("status", "")).lower()
        progress = data.get("progress", 0)

        print("当前状态:", status, "进度:", progress)

        if status == "completed":
            return data.get("url") or data.get("output") or data.get("video_url")

        if status == "failed":
            raise Exception(data.get("fail_reason") or data)

        time.sleep(interval)

    raise TimeoutError("任务超时")
```

## 返回结果字段

完成后常见视频地址字段可能是：

```text
url
output
video_url
```

建议代码里兼容这三个字段。

## 使用建议

- 视频任务耗时比图片更长，建议创建任务超时设置为 300 秒，轮询总超时设置为 600 秒。
- 轮询间隔建议 5 秒左右，不建议过于频繁。
- 如果创建失败，先打印状态码和完整返回内容。
- 如果完成但没有视频地址，检查返回字段是否为 `url`、`output` 或 `video_url`。
