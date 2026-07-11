# HappyHorse 视频生成

HappyHorse 视频模型使用 `/v1/videos` 协议，当前主要用于文生视频。它虽然复用 OpenAI 兼容的基础地址和鉴权方式，但按独立模型维护。

## 接口地址

```text
POST https://fullstacklab.xyz/v1/videos
GET  https://fullstacklab.xyz/v1/videos/{task_id}
```

## 模型列表

| 模型 | 能力 |
| --- | --- |
| happyhorse-1.0-t2v | 文生视频 |

## 请求参数

HappyHorse 使用嵌套参数，`prompt` 建议同时放在顶层和 `input.prompt` 中，便于兼容不同上游格式。

| 参数 | 类型 | 说明 |
| --- | --- | --- |
| model | string | 模型名，例如 `happyhorse-1.0-t2v` |
| prompt | string | 顶层视频提示词 |
| input.prompt | string | 嵌套视频提示词 |
| parameters.resolution | string | 分辨率，例如 `720P` |
| parameters.ratio | string | 视频比例，例如 `16:9` |
| parameters.duration | integer | 视频时长 |

## Shell 示例

```bash
curl "https://fullstacklab.xyz/v1/videos" \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "happyhorse-1.0-t2v",
    "prompt": "微型纸板城市在夜晚亮起灯光",
    "input": {
      "prompt": "微型纸板城市在夜晚亮起灯光"
    },
    "parameters": {
      "resolution": "720P",
      "ratio": "16:9",
      "duration": 5
    }
  }'
```

## Python 示例

```python
import requests

response = requests.post(
    "https://fullstacklab.xyz/v1/videos",
    headers={"Authorization": "Bearer YOUR_API_KEY"},
    json={
        "model": "happyhorse-1.0-t2v",
        "prompt": "微型纸板城市在夜晚亮起灯光",
        "input": {
            "prompt": "微型纸板城市在夜晚亮起灯光",
        },
        "parameters": {
            "resolution": "720P",
            "ratio": "16:9",
            "duration": 5,
        },
    },
    timeout=300,
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

public class HappyHorseVideoExample {
    public static void main(String[] args) throws Exception {
        String body = """
            {
              "model": "happyhorse-1.0-t2v",
              "prompt": "微型纸板城市在夜晚亮起灯光",
              "input": {
                "prompt": "微型纸板城市在夜晚亮起灯光"
              },
              "parameters": {
                "resolution": "720P",
                "ratio": "16:9",
                "duration": 5
              }
            }
            """;
        HttpRequest request = HttpRequest.newBuilder()
            .uri(URI.create("https://fullstacklab.xyz/v1/videos"))
            .header("Authorization", "Bearer YOUR_API_KEY")
            .header("Content-Type", "application/json")
            .POST(HttpRequest.BodyPublishers.ofString(body))
            .build();
        System.out.println(HttpClient.newHttpClient()
            .send(request, HttpResponse.BodyHandlers.ofString()).body());
    }
}
```

## 结果轮询

创建成功后使用 `GET /v1/videos/{task_id}` 轮询。完成后兼容读取 `url`、`video_url`、`result_url` 或 `metadata.result_url`。
