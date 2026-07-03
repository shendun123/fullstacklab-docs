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

## 请求示例

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

## 常用参数

| 参数 | 类型 | 必填 | 说明 |
| --- | --- | --- | --- |
| model | string | 是 | 模型名称，以后台可用模型为准 |
| messages | array | 是 | 对话消息列表 |
| temperature | number | 否 | 采样温度，部分模型可能不支持 |
| max_tokens | integer | 否 | 最大输出 token 数 |
| stream | boolean | 否 | 是否流式输出 |

## 返回结果

成功时通常返回 `choices[0].message.content`。  
如果使用流式输出，会按 SSE 分块返回。
