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

## 请求示例

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

## 常用参数

| 参数 | 类型 | 必填 | 说明 |
| --- | --- | --- | --- |
| model | string | 是 | Claude 模型名称 |
| max_tokens | integer | 是 | 最大输出 token 数 |
| messages | array | 是 | 消息列表 |
| system | string | 否 | 系统提示词 |
| temperature | number | 否 | 采样温度 |
| stream | boolean | 否 | 是否流式输出 |

## 注意事项

Claude Code 配置时要区分：

```text
ANTHROPIC_API_KEY
ANTHROPIC_BASE_URL
```

Base URL 建议以平台后台说明为准。
