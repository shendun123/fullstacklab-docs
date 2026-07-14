# Gemini 图像生成

Gemini 图像生成支持两种请求格式：Gemini 原生 `generateContent` 格式，以及 OpenAI Chat Completions 兼容格式。两种方式都是同步请求，但请求体和图片返回位置不同。

| 请求格式 | 接口 | 适用场景 | 图片返回位置 |
| --- | --- | --- | --- |
| Gemini 原生格式 | `/v1beta/models/{model}:generateContent` | 已按 Gemini `contents/parts` 接入的程序 | `candidates[].content.parts[]` 等原生响应字段 |
| OpenAI 兼容格式 | `/v1/chat/completions` | Chatbox、LibreChat、OneAPI/NewAPI 等兼容客户端 | `choices[0].message.content` 中的 Markdown 图片 |

> 2026-07-14 本轮实际生成验证覆盖的是 OpenAI 兼容请求。原生部分用于说明网关支持的 Gemini 请求结构，具体模型权限仍以当前 Key 的 `/v1/models` 返回和渠道配置为准。

## 一、Gemini 原生格式请求

### 接口地址

```text
POST https://fullstacklab.xyz/v1beta/models/{model}:generateContent
```

### 模型说明

原生接口的 `model` 应使用 `GET /v1/models` 返回且当前 Key 有权限的 Gemini 图像模型。下面示例使用不带 `preview` 后缀的 `gemini-3.1-flash-image`，避免继续引用本轮没有验证成功的旧 Preview 模型名。

### 请求参数

| 参数 | 类型 | 必填 | 说明 |
| --- | --- | --- | --- |
| contents | array | 是 | 对话内容列表 |
| contents[].parts[].text | string | 是 | 图片提示词 |
| contents[].parts[].inlineData | object | 否 | 纯 Base64 图片，包含 `mimeType` 和 `data` |
| contents[].parts[].fileData | object | 否 | 公网图片 URL，包含 `mimeType` 和 `fileUri` |
| generationConfig.responseModalities | array | 是 | 图片生成传 `["IMAGE"]` |
| generationConfig.imageConfig.aspectRatio | string | 否 | 图片宽高比 |
| generationConfig.imageConfig.imageSize | string | 否 | 常见值为 `1K`、`2K`、`4K`，是否支持取决于模型和渠道 |

通用比例包括 `1:1`、`2:3`、`3:2`、`3:4`、`4:3`、`4:5`、`5:4`、`9:16`、`16:9`、`21:9`。快速版还支持 `1:4`、`4:1`、`1:8`、`8:1`。

### Shell 示例

```bash
curl "https://fullstacklab.xyz/v1beta/models/gemini-3.1-flash-image:generateContent" \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "contents": [{
      "role": "user",
      "parts": [{"text": "未来城市日落天际线，电影光效"}]
    }],
    "generationConfig": {
      "responseModalities": ["IMAGE"],
      "imageConfig": {
        "aspectRatio": "16:9",
        "imageSize": "2K"
      }
    }
  }'
```

### Python 示例

```python
import requests

response = requests.post(
    "https://fullstacklab.xyz/v1beta/models/"
    "gemini-3.1-flash-image:generateContent",
    headers={"Authorization": "Bearer YOUR_API_KEY"},
    json={
        "contents": [{
            "role": "user",
            "parts": [{"text": "未来城市日落天际线，电影光效"}],
        }],
        "generationConfig": {
            "responseModalities": ["IMAGE"],
            "imageConfig": {"aspectRatio": "16:9", "imageSize": "2K"},
        },
    },
    timeout=120,
)
response.raise_for_status()
print(response.json())
```

### Java 示例

```java
import java.net.URI;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;

public class GeminiImageExample {
    public static void main(String[] args) throws Exception {
        String body = """
            {
              "contents": [{
                "role": "user",
                "parts": [{"text": "未来城市日落天际线，电影光效"}]
              }],
              "generationConfig": {
                "responseModalities": ["IMAGE"],
                "imageConfig": {"aspectRatio": "16:9", "imageSize": "2K"}
              }
            }
            """;
        HttpRequest request = HttpRequest.newBuilder()
            .uri(URI.create("https://fullstacklab.xyz/v1beta/models/"
                + "gemini-3.1-flash-image:generateContent"))
            .header("Authorization", "Bearer YOUR_API_KEY")
            .header("Content-Type", "application/json")
            .POST(HttpRequest.BodyPublishers.ofString(body))
            .build();
        System.out.println(HttpClient.newHttpClient()
            .send(request, HttpResponse.BodyHandlers.ofString()).body());
    }
}
```

### 原生返回结果

标准 Gemini 原生响应通常将图片放在 `parts[].inlineData` 中：

```text
candidates[0].content.parts[n].inlineData.mimeType
candidates[0].content.parts[n].inlineData.data
```

部分网关渠道也可能返回 `parts[].image_url.url` 或 `data[0].url`。调用方应先打印一次完整响应，再按实际字段读取；URL 可能有有效期，建议及时下载或转存。无论文生图还是图生图，输入 `parts` 中都应包含提示词 `text`。

## 二、OpenAI Chat Completions 兼容请求

此方式适合只支持 OpenAI Chat Completions 的客户端。后台将最近一条用户消息作为生图提示词，并在 `choices[0].message.content` 中返回 Markdown 图片。

### 接口地址

```text
POST https://fullstacklab.xyz/v1/chat/completions
```

### 实测可用模型

| 模型 | 本轮实测返回 | 结果 |
| --- | --- | --- |
| `gemini-3.1-flash-image` | Markdown 公网图片 URL | 成功 |
| `gemini-3.1-pro-image` | Markdown 公网图片 URL | 成功 |
| `gemini-3.0-pro-image` | Markdown 公网图片 URL | 成功 |
| `gemini-2.5-flash-image` | Markdown Base64 Data URI | 成功 |

本轮请求均成功生成 `1024x1024` PNG 图片。该尺寸是本轮测试结果，不代表所有模型和参数组合只能输出这一尺寸。

### 请求参数

| 参数 | 类型 | 必填 | 说明 |
| --- | --- | --- | --- |
| `model` | string | 是 | 使用上表中已验证的兼容模型名 |
| `messages` | array | 是 | OpenAI 消息数组，生图提示词放在 `role: user` 的消息中 |
| `stream` | boolean | 否 | 建议设为 `false`，一次性取得完整图片内容 |

本轮只验证了以上核心字段。`size`、`resolution`、`image_url`、`image_urls` 等扩展字段未在该兼容接口上逐项验证，不在本节承诺其兼容性。

### Shell 示例

```bash
curl -X POST "https://fullstacklab.xyz/v1/chat/completions" \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "gemini-3.1-flash-image",
    "messages": [
      {
        "role": "user",
        "content": "未来城市日落天际线，电影光效，画面清晰"
      }
    ],
    "stream": false
  }'
```

### Python 示例

下面的代码同时兼容公网 URL 和 `gemini-2.5-flash-image` 实测返回的 Base64 Data URI。

```python
import base64
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
        "model": "gemini-3.1-flash-image",
        "messages": [
            {
                "role": "user",
                "content": "未来城市日落天际线，电影光效，画面清晰",
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

image_source = content[len(prefix):-1]

if image_source.startswith("data:image/"):
    metadata, encoded = image_source.split(",", 1)
    extension = metadata.split("/")[1].split(";")[0]
    output = Path(f"gemini-image.{extension}")
    output.write_bytes(base64.b64decode(encoded))
else:
    image_response = requests.get(image_source, timeout=120)
    image_response.raise_for_status()
    output = Path("gemini-image.png")
    output.write_bytes(image_response.content)

print("图片来源:", image_source[:200])
print("图片已保存:", output.resolve())
```

### 兼容响应示例

公网图片 URL：

```json
{
  "choices": [
    {
      "message": {
        "role": "assistant",
        "content": "![image](https://example.com/img/generated.png)"
      }
    }
  ]
}
```

Base64 Data URI：

```json
{
  "choices": [
    {
      "message": {
        "role": "assistant",
        "content": "![image](data:image/png;base64,iVBORw0KGgo...)"
      }
    }
  ]
}
```

客户端不能只判断 `http://` 或 `https://`，还需要兼容 `data:image/`。同步生图通常需要几十秒，客户端超时时间建议至少设置为 180 秒；遇到 `502`、`503` 或 `524` 时，可有限重试 1 至 2 次。

## 三、两种请求格式不能混用

- 原生接口使用 `contents[].parts[]` 和 `generationConfig`。
- OpenAI 兼容接口使用 `messages[]` 和 `stream`。
- 原生响应从 `candidates[].content.parts[]` 读取。
- OpenAI 兼容响应从 `choices[0].message.content` 读取 Markdown 图片。
- 模型出现在 `/v1/models` 中不等于当前 Key 对所有请求格式都有权限，正式接入前应按实际使用的接口完成一次测试。
