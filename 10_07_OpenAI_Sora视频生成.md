# OpenAI Sora 视频生成

Sora 使用异步任务接口：先创建任务，再根据任务 ID 查询结果。

## 接口地址

```text
POST https://fullstacklab.xyz/v1/videos
GET  https://fullstacklab.xyz/v1/videos/{task_id}
```

## 支持模型

| 模型 | 说明 |
| --- | --- |
| sora-2 | 后台兼容模型，支持时长参数时以后台说明为准 |
| sora-2-12s | 12 秒模型，横竖屏由 `size` 决定 |

模型名称以 `GET /v1/models` 返回值为准。

## 请求参数

| 参数 | 类型 | 必填 | 说明 |
| --- | --- | --- | --- |
| model | string | 是 | Sora 模型名 |
| prompt | string | 是 | 视频提示词 |
| size | string | 是 | 如 `1920x1080` 或 `1080x1920` |
| images | string[] | 否 | JSON 请求中的公网参考图 URL，通常取第一张 |
| input_reference | file | 否 | multipart 请求中的本地参考图 |

`images` 与 `input_reference` 二选一，不要同时传。

## Shell 示例

```bash
curl "https://fullstacklab.xyz/v1/videos" \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "sora-2-12s",
    "prompt": "产品广告，镜头缓慢推进，柔和棚拍光",
    "size": "1920x1080",
    "images": ["https://example.com/reference.jpg"]
  }'
```

## Python 示例

```python
import requests

response = requests.post(
    "https://fullstacklab.xyz/v1/videos",
    headers={"Authorization": "Bearer YOUR_API_KEY"},
    json={
        "model": "sora-2-12s",
        "prompt": "产品广告，镜头缓慢推进，柔和棚拍光",
        "size": "1920x1080",
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

public class SoraExample {
    public static void main(String[] args) throws Exception {
        String body = """
            {
              "model": "sora-2-12s",
              "prompt": "产品广告，镜头缓慢推进，柔和棚拍光",
              "size": "1920x1080"
            }
            """;

        HttpRequest request = HttpRequest.newBuilder()
            .uri(URI.create("https://fullstacklab.xyz/v1/videos"))
            .header("Authorization", "Bearer YOUR_API_KEY")
            .header("Content-Type", "application/json")
            .POST(HttpRequest.BodyPublishers.ofString(body))
            .build();

        HttpResponse<String> response = HttpClient.newHttpClient()
            .send(request, HttpResponse.BodyHandlers.ofString());
        System.out.println(response.body());
    }
}
```

## 查询结果

```bash
curl "https://fullstacklab.xyz/v1/videos/TASK_ID" \
  -H "Authorization: Bearer YOUR_API_KEY"
```

状态包括 `queued`、`processing` / `in_progress`、`completed`、`failed`。完成后从 `video_url`、`url` 或 `result_url` 获取视频。

## 注意事项

- `size` 建议明确传入，宽大于高为横屏，高大于宽为竖屏。
- 参考图 URL 必须是公网可访问的图片直链。
- 建议每 5 至 10 秒轮询一次，总超时不少于 600 秒。
