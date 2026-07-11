# Gemini Veo / Omni 视频生成

Veo / Omni 使用异步任务接口，支持文生视频、参考图生视频和部分视频修改能力。

## 接口地址

```text
POST https://fullstacklab.xyz/v1/videos
GET  https://fullstacklab.xyz/v1/videos/{task_id}
```

## 支持模型

| 模型 | 说明 |
| --- | --- |
| veo_3_1-fast | Veo 3.1 Fast 示例模型 |
| veo3.1-fast | 旧版兼容写法 |
| omni_flash-10s | 10 秒、720p，支持最多 7 项参考素材 |

实际名称以 `GET /v1/models` 为准。

## 请求参数

| 参数 | 类型 | 必填 | 说明 |
| --- | --- | --- | --- |
| model | string | 是 | 模型名 |
| prompt | string | 是 | 视频提示词 |
| size | string | 否 | 如 `1280x720`、`720x1280` |
| seconds | string/number | 否 | Veo 通道时长 |
| images | string[] | 否 | JSON 请求的图片/视频 URL 或 Data URL |
| input_reference | file/string | 否 | multipart 请求的参考文件或 URL，可重复 |

JSON 使用 `images`；multipart 使用重复的 `input_reference`，不要混用。Omni 最多 7 项参考素材，暂不支持首尾帧模式。

## Shell 示例

```bash
curl "https://fullstacklab.xyz/v1/videos" \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "omni_flash-10s",
    "prompt": "根据参考图生成电影感运镜",
    "size": "1280x720",
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
        "model": "veo_3_1-fast",
        "prompt": "未来城市夜景，霓虹灯，电影感运镜",
        "size": "1280x720",
        "seconds": "8",
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

public class VeoExample {
    public static void main(String[] args) throws Exception {
        String body = """
            {
              "model": "veo_3_1-fast",
              "prompt": "未来城市夜景，霓虹灯，电影感运镜",
              "size": "1280x720",
              "seconds": "8"
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

## 查询与结果

使用 `GET /v1/videos/{task_id}` 查询。状态为 `completed` 后读取 `video_url`、`url` 或 `result_url`。建议每 5 至 10 秒轮询一次。

## 注意事项

- URL 必须是直接返回图片或视频文件的公网直链。
- JSON 无法直接上传本地视频；本地文件请使用 multipart。
- 参考素材越多，上传和处理时间越长。
