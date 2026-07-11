# Anthropic Messages API

适合 Claude / Claude Code 风格的消息接口。

## 请求地址

```text
POST https://fullstacklab.xyz/v1/messages
```

## 请求头

```text
Authorization: Bearer YOUR_API_KEY
Content-Type: application/json
anthropic-version: 2023-06-01
```

也可以使用 `x-api-key: YOUR_API_KEY`，但建议统一使用 Bearer Token。

## 支持模型和能力

`model` 以后台实际可用 Claude 模型为准。常见示例：

| 模型 | 说明 |
| --- | --- |
| claude-sonnet-4-20250514 | Claude Sonnet 4 示例模型 |
| claude-3-opus-20240229 | 示例中的 Claude 3 Opus 模型名 |

文档中该接口支持 `system`、`tools`、`tool_choice`、`thinking`、`metadata`、图片内容块和工具结果内容块。

## Shell 示例

```bash
curl "https://fullstacklab.xyz/v1/messages" \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "anthropic-version: 2023-06-01" \
  -d '{
    "model": "claude-sonnet-4-20250514",
    "max_tokens": 1024,
    "messages": [
      {"role": "user", "content": "Hello"}
    ]
  }'
```

## Python 示例

```python
import requests

api_key = "YOUR_API_KEY"
payload = {
    "model": "claude-sonnet-4-20250514",
    "max_tokens": 1024,
    "messages": [
        {"role": "user", "content": "Hello"}
    ]
}

res = requests.post(
    "https://fullstacklab.xyz/v1/messages",
    headers={
        "Authorization": f"Bearer {api_key}",
        "Content-Type": "application/json",
        "anthropic-version": "2023-06-01",
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

public class ClaudeMessagesExample {
    public static void main(String[] args) throws Exception {
        String apiKey = "YOUR_API_KEY";
        String body = """
            {
              "model": "claude-sonnet-4-20250514",
              "max_tokens": 1024,
              "messages": [
                {"role": "user", "content": "Hello"}
              ]
            }
            """;

        HttpRequest request = HttpRequest.newBuilder()
            .uri(URI.create("https://fullstacklab.xyz/v1/messages"))
            .header("Authorization", "Bearer " + apiKey)
            .header("Content-Type", "application/json")
            .header("anthropic-version", "2023-06-01")
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
| model | string | 是 | Claude 模型名称 |
| max_tokens | integer | 是 | 最大输出 token 数 |
| messages | array | 是 | 消息列表 |
| system | string | 否 | 系统提示词 |
| temperature | number | 否 | 采样温度 |
| stream | boolean | 否 | 是否流式输出 |
| top_p | number | 否 | 核采样参数 |
| top_k | integer | 否 | 采样候选数量 |
| stop_sequences | array | 否 | 停止序列 |
| tools | array | 否 | Claude 工具定义 |
| tool_choice | object | 否 | 工具选择 |
| thinking | object | 否 | 思考配置，例如 `enabled` 和预算 token |

## 注意事项

Claude Code 配置时要区分：

```text
ANTHROPIC_API_KEY
ANTHROPIC_BASE_URL
```

Base URL 建议以平台后台说明为准。
