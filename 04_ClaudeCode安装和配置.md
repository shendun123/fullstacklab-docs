# Claude Code 安装和配置

本节适合想安装 Claude Code，并通过 全栈实验室 接入 Claude Code 的用户。

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

推荐优先使用 ccswitch 导入配置。如果需要手动配置，准备：

- API Key：从 全栈实验室 后台获取
- Base URL：`https://fullstacklab.xyz`

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
    "ANTHROPIC_BASE_URL": "https://fullstacklab.xyz",
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

常见问题包括 npm 不存在、安装慢、权限不足、命令不在 PATH、Key 错误、Base URL 错误、JSON 格式错误或工具没有完全重启。
