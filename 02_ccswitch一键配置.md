# ccswitch 一键配置

ccswitch 适合已经安装好 Claude Code 或 Codex 的用户。它可以把 全栈实验室 后台里的 API Key 和 Base URL 导入到本地工具配置中。

使用前需要确认：

- 已安装 ccswitch
- 已安装 Claude Code 或 Codex
- 已登录 全栈实验室 后台
- 后台已有可用 API Key

官方入口：

- `https://ccswitch.io/`
- `https://github.com/farion1231/cc-switch`

常见流程是进入后台的 API 密钥页面，选择要使用的 Key，点击类似“导入到 CCS”的按钮。浏览器可能会询问是否打开 CC Switch，确认来源无误后允许打开。

导入时检查供应商名称、API 端点和打码后的 Key。Base URL 应为：

```text
https://fullstacklab.xyz
```

### 手动添加自定义配置的位置说明

如果没有自动导入，或者导入后需要手动检查配置，可以在 ccswitch 里按下面步骤添加：

1. 打开 ccswitch。
2. 在供应商列表中，先选择 `Claude Desktop Official` 这个图标。
3. 点击界面最左侧的 `+` 加号。
4. 选择 `自定义配置`。
5. 在配置页面填写以下内容：

```text
请求地址 / Base URL：https://fullstacklab.xyz
API Key：填写全栈实验室后台生成的 API Key
```

6. 确认请求地址没有多余空格，API Key 没有漏复制。
7. 点击 `添加` 或 `保存`。
8. 添加成功后，回到供应商列表，确认当前启用的是刚刚新增的自定义配置。

注意：API Key 属于敏感信息，不要截图公开完整 Key，也不要把 Key 发到公开群或文档里。

导入完成后，完全关闭 Claude Code、Codex 和相关终端，再重新打开。可以发送测试消息：

```text
请回复“配置已生效”。
```

如果能收到正常回复，说明配置已生效。

常见问题：

| 现象 | 处理方式 |
| --- | --- |
| 浏览器没有唤起 ccswitch | 确认 ccswitch 已安装，并检查浏览器是否拦截外部应用 |
| 导入后没有变化 | 完全退出 Claude Code、Codex 和终端后重开 |
| 认证失败 | 回后台重新复制或重新导入 API Key |
| 连接失败 | 检查 Base URL 是否为 `https://fullstacklab.xyz` |
