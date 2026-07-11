# Gemini 原生图像生成

Gemini 原生图片接口使用 `generateContent` 同步返回，支持文生图和图生图。

## 接口地址

```text
POST https://fullstacklab.xyz/v1beta/models/{model}:generateContent
```

## 支持模型

| 模型 | 说明 |
| --- | --- |
| gemini-3-pro-image-preview | 专业版，画质和一致性优先 |
| gemini-3.1-flash-image-preview | 快速版，支持更多比例和分辨率选项 |

## 请求参数

| 参数 | 类型 | 必填 | 说明 |
| --- | --- | --- | --- |
| contents | array | 是 | 对话内容列表 |
| contents[].parts[].text | string | 是 | 图片提示词 |
| contents[].parts[].inlineData | object | 否 | 纯 Base64 图片，包含 `mimeType` 和 `data` |
| contents[].parts[].fileData | object | 否 | 公网图片 URL，包含 `mimeType` 和 `fileUri` |
| generationConfig.responseModalities | array | 是 | 图片生成传 `["IMAGE"]` |
| generationConfig.imageConfig.aspectRatio | string | 否 | 图片宽高比 |
| generationConfig.imageConfig.imageSize | string | 否 | `1K`、`2K`、`4K`，仅快速版支持 |

通用比例包括 `1:1`、`2:3`、`3:2`、`3:4`、`4:3`、`4:5`、`5:4`、`9:16`、`16:9`、`21:9`。快速版还支持 `1:4`、`4:1`、`1:8`、`8:1`。

## Shell 示例

```bash
curl "https://fullstacklab.xyz/v1beta/models/gemini-3.1-flash-image-preview:generateContent" \
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

## Python 示例

```python
import requests

response = requests.post(
    "https://fullstacklab.xyz/v1beta/models/"
    "gemini-3.1-flash-image-preview:generateContent",
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

## Java 示例

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
                + "gemini-3.1-flash-image-preview:generateContent"))
            .header("Authorization", "Bearer YOUR_API_KEY")
            .header("Content-Type", "application/json")
            .POST(HttpRequest.BodyPublishers.ofString(body))
            .build();
        System.out.println(HttpClient.newHttpClient()
            .send(request, HttpResponse.BodyHandlers.ofString()).body());
    }
}
```

## 返回结果

图片地址通常位于：

```text
candidates[0].content.parts[0].image_url.url
data[0].url
```

返回链接可能有有效期，请及时下载或转存。无论文生图还是图生图，`parts` 中都应包含 `text`。
