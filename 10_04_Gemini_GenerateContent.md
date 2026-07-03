# Gemini GenerateContent

适合 Gemini 兼容协议调用。

## 请求地址

```text
POST https://fullstacklab.xyz/v1beta/models/{model}:generateContent
```

示例：

```text
POST https://fullstacklab.xyz/v1beta/models/gemini-2.5-pro:generateContent
```

## 请求头

```text
Authorization: Bearer YOUR_API_KEY
Content-Type: application/json
```

## 请求示例

```bash
curl "https://fullstacklab.xyz/v1beta/models/gemini-2.5-pro:generateContent" \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -d '{
    "contents": [
      {
        "parts": [
          {"text": "你好，请回复一句测试"}
        ]
      }
    ],
    "generationConfig": {
      "temperature": 0.7,
      "maxOutputTokens": 1024
    }
  }'
```

## 常用参数

| 参数 | 类型 | 必填 | 说明 |
| --- | --- | --- | --- |
| contents | array | 是 | 内容列表 |
| contents[].parts[].text | string | 是 | 文本输入 |
| generationConfig.temperature | number | 否 | 采样温度 |
| generationConfig.maxOutputTokens | number | 否 | 最大输出 token 数 |
| systemInstruction | object | 否 | 系统指令 |

## 注意事项

不同 Gemini SDK 对自定义 Base URL 的支持不同。如果 SDK 不方便配置代理地址，可以优先使用 REST 请求。
