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

### 具体配置步骤

不同版本的 Cherry Studio 菜单名称可能略有不同，但整体配置逻辑一致，可以按下面步骤操作：

1. 打开 Cherry Studio。
2. 点击左下角或侧边栏的 `设置`。
3. 找到 `模型服务`、`模型提供商`、`API 配置`、`OpenAI Compatible`、`OpenAI 兼容` 或 `自定义接口` 入口。
4. 点击 `添加`、`新增供应商` 或 `添加模型服务`。
5. 供应商类型选择 `OpenAI Compatible`、`OpenAI 兼容` 或 `自定义 OpenAI 接口`。
6. 在配置页面填写以下内容：

```text
供应商名称：全栈实验室
API 地址 / Base URL：https://fullstacklab.xyz/v1
API Key：填写你在全栈实验室后台创建的真实 Key
默认模型：填写后台可用模型名，例如 gpt-5.5
```

7. 保存配置。
8. 回到模型列表，确认已经选择刚刚添加的 `全栈实验室` 供应商。
9. 新建对话，选择对应模型，发送测试消息：

```text
请回复“配置已生效”。
```

如果能正常回复，说明 Cherry Studio 已经成功连接到全栈实验室。

注意：API Key 只需要填写你自己在平台后台创建的真实 Key，不要填写示例 Key，也不要把完整 Key 截图发给别人。


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
