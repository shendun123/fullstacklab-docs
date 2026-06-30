# Codex 安装和配置

本节适合想安装 Codex，并通过 全栈实验室 接入 Codex 的用户。

相关入口：

- Codex App 页面：`https://openai.com/codex/for-work/`
- Codex 介绍页：`https://openai.com/index/introducing-the-codex-app/`
- Codex 文档：`https://developers.openai.com/codex`
- Codex npm 包：`https://www.npmjs.com/package/@openai/codex`
- Node.js：`https://nodejs.org/en/download`

Codex CLI 依赖 Node.js 和 npm。先检查：

```bash
node -v
npm -v
```

安装 Codex CLI：

```bash
npm install -g @openai/codex
```

验证安装：

```bash
codex --version
```

推荐优先使用 ccswitch 导入配置。如果手动配置，需要准备：

- API Key：从 全栈实验室 后台获取
- Base URL：`https://api.fullstacklab.xyz`
- 模型名：从后台模型列表获取

不要把 API Key 写进项目仓库、README、截图或公开聊天记录。

Codex 配置目录常见路径：

```text
Windows:
%USERPROFILE%\.codex\config.toml
%USERPROFILE%\.codex\auth.json

macOS/Linux:
~/.codex/config.toml
~/.codex/auth.json
```

`config.toml` 示例：

```toml
model_provider = "custom"
model = "gpt-5.4"

[model_providers.custom]
name = "custom"
base_url = "https://api.fullstacklab.xyz"
wire_api = "responses"
requires_openai_auth = true
```

`auth.json` 示例：

```json
{
  "OPENAI_API_KEY": "sk-xxxxxxxxxxxxxxxx"
}
```

保存后完全关闭 Codex 和终端，再重新打开：

```bash
codex login status
codex
```

进入后发送：

```text
请回复“配置已生效”。
```

如果能正常回复，说明 Codex 已接入 全栈实验室。

常见问题包括 npm 不存在、权限不足、命令不在 PATH、配置目录写错、仍走默认配置、401 鉴权失败、Base URL 错误、TOML/JSON 格式错误以及工具没有完全重启。
