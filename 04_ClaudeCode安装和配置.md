# Claude Code 安装和配置

本节适合想安装 Claude Code，并通过 全栈实验室 接入 Claude Code 的用户。推荐先安装 Claude Code，再使用 ccswitch 一键配置；如果一键配置不可用，也可以按下方方式手动配置。

相关入口：

- Claude Desktop 下载页：`https://claude.com/download`
- Claude Desktop 帮助页：`https://support.claude.com/en/articles/10065433-installing-claude-for-desktop`
- Claude Code 文档：`https://code.claude.com/docs`
- Claude Code npm 包：`https://www.npmjs.com/package/@anthropic-ai/claude-code`
- Node.js：`https://nodejs.org/en/download`

Claude Code 依赖 Node.js 和 npm。先检查：

```bash
node -v
npm -v
```

如果没有版本号，先安装 Node.js LTS。建议 Node.js 18 或更高版本。

安装 Claude Code：

```bash
npm install -g @anthropic-ai/claude-code
```

验证安装：

```bash
claude --version
```

## ccswitch 一键配置

ccswitch 可以把 全栈实验室 后台里的 API Key 和 Base URL 导入到本地工具配置中，适合已经安装好 Claude Code 的用户。

使用前需要确认：

- 已安装 ccswitch
- 已安装 Claude Code
- 已登录 全栈实验室 后台
- 后台已有可用 API Key

官方入口：

- `https://ccswitch.io/`
- `https://github.com/farion1231/cc-switch`

常见流程是进入后台的 API 密钥页面，选择要使用的 Key，点击类似“导入到 CCS”的按钮。浏览器可能会询问是否打开 CC Switch，确认来源无误后允许打开。

导入时检查供应商名称、API 端点和打码后的 Key。Base URL 应为：

```text
https://api.fullstacklab.xyz
```

导入完成后，完全关闭 Claude Code 和相关终端，再重新打开。可以发送测试消息：

```text
请回复“配置已生效”。
```

如果能收到正常回复，说明配置已生效。

## 手动配置

如果需要手动配置，准备：

- API Key：从 全栈实验室 后台获取
- Base URL：`https://api.fullstacklab.xyz`

Claude Code 配置文件常见路径：

```text
Windows: %USERPROFILE%\.claude\settings.json
macOS/Linux: ~/.claude/settings.json
```

如果 `settings.json` 不存在，可以先启动一次 Claude Code，或手动创建目录和文件。

示例配置：

```json
{
  "env": {
    "ANTHROPIC_API_KEY": "sk-xxxxxxxxxxxxxxxx",
    "ANTHROPIC_BASE_URL": "https://api.fullstacklab.xyz",
    "CLAUDE_AUTOCOMPACT_PCT_OVERRIDE": "78"
  }
}
```

如果文件里已有其他内容，不要清空整份文件，只把需要的 `env` 字段合并进去。

保存后完全关闭 Claude Code 和终端，再重新打开，执行：

```bash
claude
```

进入后发送：

```text
请回复“配置已生效”。
```

能回复即代表配置成功。

## 常见问题

| 现象 | 处理方式 |
| --- | --- |
| npm 不存在 | 先安装 Node.js LTS，并确认重新打开终端后能执行 `node -v` 和 `npm -v` |
| 安装慢或失败 | 检查网络环境，必要时更换 npm 镜像后重试 |
| 权限不足 | Windows 建议用管理员终端，macOS/Linux 可检查 npm 全局安装目录权限 |
| `claude` 命令不在 PATH | 重新打开终端，或检查 npm 全局 bin 目录是否在 PATH 中 |
| 浏览器没有唤起 ccswitch | 确认 ccswitch 已安装，并检查浏览器是否拦截外部应用 |
| 导入后没有变化 | 完全退出 Claude Code 和终端后重开 |
| 认证失败 | 回后台重新复制或重新导入 API Key |
| 连接失败 | 检查 Base URL 是否为 `https://api.fullstacklab.xyz` |
| JSON 格式错误 | 检查 `settings.json` 是否有多余逗号、缺少引号或括号不匹配 |
