# 视频生成

视频生成使用任务式接口：先创建视频任务，再根据任务 ID 轮询视频结果。本页覆盖 Veo、Sora 以及 Apifox 文档中可通过 `/v1/videos` 接入的国产视频模型系列。

## 支持模型

### Sora / Veo

| 模型 | 说明 |
| --- | --- |
| sora-2 | Sora 视频模型，支持 10 秒或 15 秒 |
| veo_3_1-fast | Veo 3.1 Fast，Apifox 示例模型 |
| veo3.1-fast | 兼容旧文档写法，实际以后台模型名为准 |

### 国产视频模型系列

| 模型族 | 可选模型示例 |
| --- | --- |
| Vidu | Vidu-q2、Vidu-q2-pro、Vidu-q2-turbo、Vidu-q3-pro、Vidu-q3-turbo、Vidu-template |
| Kling | Kling-1.6、Kling-2.0、Kling-2.1、Kling-2.5、Kling-2.6、Kling-3.0、Kling-3.0-Omni、Kling-O1 |
| Hailuo | Hailuo-02、Hailuo-2.3、Hailuo-2.3-fast |
| Hunyuan / Mingmou / OS / GV | Hunyuan-1.5、Mingmou-1.0、OS-2.0、GV-3.1、GV-3.1-fast |
| SV / JV | SV-1.5-pro、SV-1.0-pro、SV-1.0-pro-fast、SV-1.0-lite、JV-3.0-pro |

组合计费模型也可以直接作为 `model` 传入，例如：

- `vidu-q2-pro-reference-1080p-offpeak`
- `kling-2.6-motion-pro-1080p`
- `kling-3.0-omni-1080p-ref-audio`
- `hailuo-2.3-fast-1080p`
- `sv-1.5-pro-1080p-audio`
- `jv-3.0-pro`

## 创建任务

```text
POST https://fullstacklab.xyz/v1/videos
```

## 查询任务

```text
GET https://fullstacklab.xyz/v1/videos/{task_id}
```

## 请求头

```text
Authorization: Bearer YOUR_API_KEY
Accept: application/json
```

## 创建任务参数

| 参数 | 类型 | 示例 | 说明 |
| --- | --- | --- | --- |
| model | string | veo_3_1-fast | 视频模型 |
| prompt | string | 一个女孩在雨夜街头奔跑 | 视频描述 |
| size | string | 720x1280 | 视频尺寸 |
| seconds | string / number | 8 | 视频时长 |
| input_reference | file/string | 参考图或参考视频 | 参考素材，Sora/Veo 可用 |
| image / images | string / array | https://example.com/a.png | 国产视频模型图生视频或参考图 |
| metadata | object | output_config | 国产视频模型扩展配置 |

## 创建任务示例

### Shell

```bash
curl -X POST "https://fullstacklab.xyz/v1/videos" \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -F "model=veo_3_1-fast" \
  -F "prompt=一个电影感镜头，未来城市夜景，霓虹灯，高级质感" \
  -F "size=720x1280" \
  -F "seconds=8"
```

### Python

```python
import requests

API_KEY = "YOUR_API_KEY"
BASE_URL = "https://fullstacklab.xyz"

headers = {
    "Authorization": f"Bearer {API_KEY}",
    "Accept": "application/json"
}

payload = {
    "model": "veo_3_1-fast",
    "prompt": "一个电影感镜头，未来城市夜景，霓虹灯，高级质感",
    "size": "720x1280",
    "seconds": "8"
}

res = requests.post(
    f"{BASE_URL}/v1/videos",
    headers=headers,
    data=payload,
    timeout=300
)

print(res.status_code)
print(res.text)

task = res.json()
task_id = task.get("id") or task.get("task_id")
print("任务 ID:", task_id)
```

### Java

```java
import java.net.URI;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;

public class VideoExample {
    public static void main(String[] args) throws Exception {
        String apiKey = "YOUR_API_KEY";
        String boundary = "----FullstackLabBoundary";
        String body = "--" + boundary + "\r\n"
            + "Content-Disposition: form-data; name=\"model\"\r\n\r\n"
            + "veo_3_1-fast\r\n"
            + "--" + boundary + "\r\n"
            + "Content-Disposition: form-data; name=\"prompt\"\r\n\r\n"
            + "一个电影感镜头，未来城市夜景，霓虹灯，高级质感\r\n"
            + "--" + boundary + "\r\n"
            + "Content-Disposition: form-data; name=\"size\"\r\n\r\n"
            + "720x1280\r\n"
            + "--" + boundary + "\r\n"
            + "Content-Disposition: form-data; name=\"seconds\"\r\n\r\n"
            + "8\r\n"
            + "--" + boundary + "--\r\n";

        HttpRequest request = HttpRequest.newBuilder()
            .uri(URI.create("https://fullstacklab.xyz/v1/videos"))
            .header("Authorization", "Bearer " + apiKey)
            .header("Content-Type", "multipart/form-data; boundary=" + boundary)
            .POST(HttpRequest.BodyPublishers.ofString(body))
            .build();

        HttpResponse<String> response = HttpClient.newHttpClient()
            .send(request, HttpResponse.BodyHandlers.ofString());

        System.out.println(response.body());
    }
}
```

## 国产视频模型 JSON 示例

```bash
curl -X POST "https://fullstacklab.xyz/v1/videos" \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "Kling-3.0-Omni",
    "prompt": "镜头环绕建筑并使用希区柯克变焦",
    "seconds": "6",
    "metadata": {
      "offpeak": true,
      "file_infos": [
        {
          "type": "Url",
          "category": "Image",
          "url": "https://example.com/ref.jpg"
        }
      ],
      "output_config": {
        "resolution": "1080P",
        "aspect_ratio": "16:9",
        "audio_generation": "Enabled"
      }
    }
  }'
```

## 轮询任务示例

```python
import time
import requests

def query_task(task_id):
    url = f"https://fullstacklab.xyz/v1/videos/{task_id}"
    headers = {
        "Authorization": f"Bearer {API_KEY}",
        "Accept": "application/json"
    }
    res = requests.get(url, headers=headers, timeout=60)
    res.raise_for_status()
    return res.json()

def wait_for_result(task_id, interval=5, timeout=600):
    start = time.time()

    while time.time() - start < timeout:
        data = query_task(task_id)
        status = str(data.get("status", "")).lower()
        progress = data.get("progress", 0)

        print("当前状态:", status, "进度:", progress)

        if status == "completed":
            return data.get("url") or data.get("output") or data.get("video_url")

        if status == "failed":
            raise Exception(data.get("fail_reason") or data)

        time.sleep(interval)

    raise TimeoutError("任务超时")
```

## 返回结果字段

完成后常见视频地址字段可能是：

```text
url
output
video_url
```

建议代码里兼容这三个字段。

## 使用建议

- 视频任务耗时比图片更长，建议创建任务超时设置为 300 秒，轮询总超时设置为 600 秒。
- 轮询间隔建议 5 秒左右，不建议过于频繁。
- 如果创建失败，先打印状态码和完整返回内容。
- 如果完成但没有视频地址，检查返回字段是否为 `url`、`output` 或 `video_url`。
- 国产视频模型复杂场景建议把分辨率、比例、音频等参数放进 `metadata.output_config`。
- 动作控制类场景通常需要视频参考，只传图片可能失败。
