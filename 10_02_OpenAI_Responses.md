# OpenAI Responses API

适合使用 Responses 协议的客户端，例如部分 Codex 配置、推理模型、工具调用和新式 OpenAI 兼容接口。

## 请求地址

```text
POST https://fullstacklab.xyz/v1/responses
```

## 请求头

```text
Authorization: Bearer YOUR_API_KEY
Content-Type: application/json
```

## 支持模型和能力

`model` 以后台实际可用模型和 `/v1/models` 返回结果为准。Responses API 适合 Codex、推理模型、多轮续接和工具调用场景。

常用能力包括：

- 字符串或消息数组输入：`input`
- 系统指令：`instructions`
- 工具调用：`tools`、`tool_choice`
- 推理配置：`reasoning.effort`
- 多轮续接：`previous_response_id`
- 截断策略：`truncation`
- 流式输出：`stream`

## Shell 示例

```bash
curl -X POST "https://fullstacklab.xyz/v1/responses" \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "gpt-5.5",
    "input": "你好，请回复一句测试"
  }'
```

## Python 示例

```python
import requests

api_key = "YOUR_API_KEY"
payload = {
    "model": "gpt-5.5",
    "input": "你好，请回复一句测试",
    "reasoning": {"effort": "medium"}
}

res = requests.post(
    "https://fullstacklab.xyz/v1/responses",
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

public class ResponsesExample {
    public static void main(String[] args) throws Exception {
        String apiKey = "YOUR_API_KEY";
        String body = """
            {
              "model": "gpt-5.5",
              "input": "你好，请回复一句测试",
              "reasoning": {"effort": "medium"}
            }
            """;

        HttpRequest request = HttpRequest.newBuilder()
            .uri(URI.create("https://fullstacklab.xyz/v1/responses"))
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
| model | string | 是 | 模型名称 |
| input | string / array | 是 | 输入内容 |
| instructions | string | 否 | 系统指令 |
| max_output_tokens | integer | 否 | 最大输出 token 数 |
| temperature | number | 否 | 采样温度，部分模型可能不支持 |
| stream | boolean | 否 | 是否流式输出 |
| tools | array | 否 | 工具定义 |
| tool_choice | string / object | 否 | 工具选择 |
| reasoning | object | 否 | 推理相关配置 |
| previous_response_id | string | 否 | 续接上一轮 response |
| truncation | string | 否 | 截断策略 |

## Codex 配置提示

Codex 使用 Responses 协议时，配置中通常需要：

```toml
wire_api = "responses"
```

并且 Base URL 通常填写：

```text
https://fullstacklab.xyz/v1
```
