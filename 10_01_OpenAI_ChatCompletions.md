# OpenAI Chat Completions

适合普通聊天补全接口，兼容 OpenAI 风格的 `/v1/chat/completions`。

## 请求地址

```text
POST https://fullstacklab.xyz/v1/chat/completions
```

## 请求头

```text
Authorization: Bearer YOUR_API_KEY
Content-Type: application/json
```

## 支持模型和能力

`model` 以后台实际可用模型和 `/v1/models` 返回结果为准。常见可用于 Chat Completions 的模型包括 OpenAI 兼容聊天模型、推理模型，以及后台开放的多模态模型。

Apifox 文档中该接口支持的主要能力包括：

- 普通文本对话
- 流式输出：`stream`
- 工具调用：`tools`、`tool_choice`
- JSON 输出：`response_format`
- 推理强度：`reasoning_effort`
- 多模态输入：`image_url`、`input_audio`、`file`、`video_url`
- 多模态输出声明：`modalities`、`audio`

## Shell 示例

```bash
curl "https://fullstacklab.xyz/v1/chat/completions" \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -d '{
    "model": "gpt-5.5",
    "messages": [
      {"role": "user", "content": "你好，请回复一句测试"}
    ]
  }'
```

## Python 示例

```python
import requests

api_key = "YOUR_API_KEY"
url = "https://fullstacklab.xyz/v1/chat/completions"

payload = {
    "model": "gpt-5.5",
    "messages": [
        {"role": "user", "content": "你好，请回复一句测试"}
    ]
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
print(res.json()["choices"][0]["message"]["content"])
```

## Java 示例

```java
import java.net.URI;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;

public class ChatCompletionsExample {
    public static void main(String[] args) throws Exception {
        String apiKey = "YOUR_API_KEY";
        String body = """
            {
              "model": "gpt-5.5",
              "messages": [
                {"role": "user", "content": "你好，请回复一句测试"}
              ]
            }
            """;

        HttpRequest request = HttpRequest.newBuilder()
            .uri(URI.create("https://fullstacklab.xyz/v1/chat/completions"))
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
| model | string | 是 | 模型名称，以后台可用模型为准 |
| messages | array | 是 | 对话消息列表 |
| temperature | number | 否 | 采样温度，部分模型可能不支持 |
| max_tokens | integer | 否 | 最大输出 token 数 |
| max_completion_tokens | integer | 否 | 最大补全 token 数 |
| stream | boolean | 否 | 是否流式输出 |
| stream_options.include_usage | boolean | 否 | 流式输出时是否包含 usage |
| tools | array | 否 | 工具定义 |
| tool_choice | string / object | 否 | 工具选择 |
| response_format | object | 否 | `text`、`json_object` 或 `json_schema` |
| reasoning_effort | string | 否 | 推理强度：`low`、`medium`、`high` |

## 返回结果

成功时通常返回 `choices[0].message.content`。  
如果使用流式输出，会按 SSE 分块返回。
