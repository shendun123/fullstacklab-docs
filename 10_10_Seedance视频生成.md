# Seedance 视频生成

Seedance 使用异步视频任务接口。模型 ID 可能包含渠道前缀和规格后缀，必须以模型列表返回值为准。

## 接口地址

```text
POST https://fullstacklab.xyz/v1/videos
GET  https://fullstacklab.xyz/v1/videos/{task_id}
GET  https://fullstacklab.xyz/v1/videos/{task_id}/content
GET  https://fullstacklab.xyz/v1/models
```

## 模型名称

示例完整 ID：

```text
12:seedance-2.0-720p-fast
```

不要手工猜测数字前缀，调用前先请求 `GET /v1/models`。

## 请求参数

| 参数 | 类型 | 必填 | 说明 |
| --- | --- | --- | --- |
| model | string | 是 | 完整模型 ID |
| prompt | string | 是 | 可用 `@图片1`、`@音频1`、`@视频1` 引用素材 |
| seconds | string | 否 | 时长，如 `10` |
| size | string | 否 | `16:9`、`9:16`、`1:1`、`4:3`、`3:4` |
| metadata.mode_type | string | 否 | `text2video`、`image2video`、`frames2video` |
| metadata.resolution | string | 否 | `480p`、`720p`、`1080p` |
| metadata.images | string[] | 否 | 公网图片 URL |
| metadata.audios | string[] | 否 | 公网音频 URL，最多 3 段 |
| metadata.videos | string[] | 否 | 公网视频 URL，最多 3 段 |
| metadata.disable_audio | boolean | 否 | 是否关闭自动配音 |

模式约束：`text2video` 不可带参考图；`image2video` 至少 1 张图；`frames2video` 必须正好 2 张图。

## Shell 示例

```bash
curl "https://fullstacklab.xyz/v1/videos" \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "12:seedance-2.0-720p-fast",
    "prompt": "@图片1 中的人物向镜头走来",
    "seconds": "10",
    "size": "16:9",
    "metadata": {
      "mode_type": "image2video",
      "resolution": "720p",
      "disable_audio": true,
      "images": ["https://example.com/reference.jpg"]
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
        "model": "12:seedance-2.0-720p-fast",
        "prompt": "@图片1 中的人物向镜头走来",
        "seconds": "10",
        "size": "16:9",
        "metadata": {
            "mode_type": "image2video",
            "resolution": "720p",
            "disable_audio": True,
            "images": ["https://example.com/reference.jpg"],
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

public class SeedanceExample {
    public static void main(String[] args) throws Exception {
        String body = """
            {
              "model": "12:seedance-2.0-720p-fast",
              "prompt": "@图片1 中的人物向镜头走来",
              "seconds": "10",
              "size": "16:9",
              "metadata": {
                "mode_type": "image2video",
                "resolution": "720p",
                "disable_audio": true,
                "images": ["https://example.com/reference.jpg"]
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

## 查询与下载

通过 `GET /v1/videos/{task_id}` 查询。成功结果可能位于 `metadata.result_url`；也可调用 `GET /v1/videos/{task_id}/content` 下载视频。

## 常见错误

| 错误码 | 说明 |
| --- | --- |
| invalid_metadata | 模式与参考素材数量不匹配 |
| missing_model | 缺少模型 |
| model_mapping_failed | 模型名称无法映射 |
| insufficient_user_quota | 额度不足 |
| channel_no_available_key | 上游当前无可用通道 |
