# Cherry Studio 教程

Cherry Studio 适合想通过图形界面接入 全栈实验室 的用户。它可以作为支持 OpenAI 兼容接口的通用桌面客户端。

官方入口：

- `https://github.com/CherryHQ/cherry-studio`
- `https://github.com/CherryHQ/cherry-studio/releases`

下载时根据系统选择对应安装包：

- Windows：通常选择 `.exe` 或 `.msi`
- macOS：通常选择 `.dmg`
- Linux：通常选择 `.AppImage`、`.deb` 或 `.rpm`

安装后进入设置页面，找到模型提供商、API 配置、OpenAI Compatible、OpenAI 兼容或自定义接口等入口。

推荐填写：

```text
Base URL: https://api.fullstacklab.xyz
API Key: 使用 全栈实验室 后台生成的 Key
Organization: 如无特殊要求可留空
Default Model: 填写后台可用模型名
```

保存后新建对话并发送测试消息。如果能正常回复，说明 Cherry Studio 已连接到 全栈实验室。

常见排查：

| 现象 | 处理方式 |
| --- | --- |
| 连接失败 | 检查 Base URL 是否正确，前后不要带空格 |
| 鉴权失败 | 检查 API Key 是否复制完整 |
| 看不到模型 | 确认后台是否开通模型权限，或手动填写模型名 |
| 菜单名称不同 | 优先寻找 API、模型提供商、OpenAI 兼容、自定义接口等关键词 |
