# GPT Image 2 图像生成

本接口适合 OpenAI 风格同步生图，请求后直接返回图片 URL 或 `b64_json`。

## 请求地址

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

## Python 示例

```python
import base64
import time
from pathlib import Path
import requests

API_KEY = "YOUR_API_KEY"
BASE_URL = "https://fullstacklab.xyz/v1"

payload = {
    "model": "gpt-image-2",
    "prompt": "A cute orange cat playing with yarn, studio ghibli style",
    "n": 1,
    "size": "1024x1024"
}

headers = {
    "Authorization": f"Bearer {API_KEY}",
    "Content-Type": "application/json"
}

res = requests.post(
    f"{BASE_URL}/images/generations",
    headers=headers,
    json=payload,
    timeout=120
)
res.raise_for_status()
result = res.json()

save_dir = Path("./generated_images")
save_dir.mkdir(exist_ok=True)

for index, item in enumerate(result.get("data", []), start=1):
    if "url" in item:
        img = requests.get(item["url"], timeout=120)
        img.raise_for_status()
        (save_dir / f"image_{index}_{int(time.time())}.png").write_bytes(img.content)

    elif "b64_json" in item:
        data = base64.b64decode(item["b64_json"])
        (save_dir / f"image_{index}_{int(time.time())}.png").write_bytes(data)
```

## 排查建议

从测试脚本可以总结出几类重点排查方向：

- 连接失败：检查接口地址、端口、网络和防火墙。
- 超时失败：图片生成耗时较长，建议把超时设置到 60 秒以上，必要时提高到 120 或 300 秒。
- HTTP 错误：打印响应内容，优先看状态码和错误信息。
- 返回图片保存：兼容 `url` 和 `b64_json` 两种返回格式。
