# Gemini GenerateContent

适合 Gemini 兼容协议调用。

## 请求地址

```text
POST https://fullstacklab.xyz/v1beta/models/{model}:generateContent
```

示例：

```text
POST https://fullstacklab.xyz/v1beta/models/gemini-2.5-pro:generateContent
```

媒体识别和流式场景也可以使用：

```text
POST https://fullstacklab.xyz/v1beta/models/{model}:streamGenerateContent
```

## 请求头

```text
Authorization: Bearer YOUR_API_KEY
Content-Type: application/json
```

## 支持模型和能力

`model` 以后台实际可用 Gemini 模型为准。常见示例：

| 模型 | 说明 |
| --- | --- |
| gemini-2.5-pro | 文本生成示例模型 |
| gemini-3-flash-preview | Apifox 媒体识别示例模型 |

支持文本、图片、PDF、音频、视频等内容块。媒体识别接口建议使用 `inlineData` 传 base64；Apifox 标注该媒体识别请求不支持 `fileData.fileUri` 或 File API。

## Shell 示例

```bash
curl "https://fullstacklab.xyz/v1beta/models/gemini-2.5-pro:generateContent" \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -d '{
    "contents": [
      {
        "parts": [
          {"text": "你好，请回复一句测试"}
        ]
      }
    ],
    "generationConfig": {
      "temperature": 0.7,
      "maxOutputTokens": 1024
    }
  }'
```

## Python 示例

```python
import requests

api_key = "YOUR_API_KEY"
url = "https://fullstacklab.xyz/v1beta/models/gemini-2.5-pro:generateContent"

payload = {
    "contents": [
        {
            "role": "user",
            "parts": [{"text": "你好，请回复一句测试"}]
        }
    ],
    "generationConfig": {
        "temperature": 0.7,
        "maxOutputTokens": 1024
    }
}

res = requests.post(
    url,
    headers={
        "Authorization": f"Bearer {api_key}",
        "Content-Type": "application/json",
    },
    json=payload,
    timeout=60,
)
res.raise_for_status()
print(res.text)
```

## Java 示例

```java
import java.net.URI;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;

public class GeminiGenerateContentExample {
    public static void main(String[] args) throws Exception {
        String apiKey = "YOUR_API_KEY";
        String body = """
            {
              "contents": [
                {
                  "role": "user",
                  "parts": [
                    {"text": "你好，请回复一句测试"}
                  ]
                }
              ],
              "generationConfig": {
                "temperature": 0.7,
                "maxOutputTokens": 1024
              }
            }
            """;

        HttpRequest request = HttpRequest.newBuilder()
            .uri(URI.create("https://fullstacklab.xyz/v1beta/models/gemini-2.5-pro:generateContent"))
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

## 常用参数

| 参数 | 类型 | 必填 | 说明 |
| --- | --- | --- | --- |
| contents | array | 是 | 内容列表 |
| contents[].parts[].text | string | 是 | 文本输入 |
| contents[].parts[].inlineData | object | 否 | base64 媒体内容，包含 `mimeType` 和 `data` |
| generationConfig.temperature | number | 否 | 采样温度 |
| generationConfig.maxOutputTokens | number | 否 | 最大输出 token 数 |
| systemInstruction | object | 否 | 系统指令 |

## 注意事项

不同 Gemini SDK 对自定义 Base URL 的支持不同。如果 SDK 不方便配置代理地址，可以优先使用 REST 请求。
