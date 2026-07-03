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



## Windows 版本具体配置步骤

下面是 Windows 用户最常用的配置方式，适合使用 PowerShell 手动写入 Codex 配置文件。

### 第一步：打开 PowerShell

在 Windows 开始菜单搜索：

```text
PowerShell
```

然后打开 PowerShell。建议使用普通 PowerShell 即可，不一定需要管理员权限。

### 第二步：创建 Codex 配置目录和文件

逐步输入以下命令：

```powershell
New-Item -ItemType Directory -Force "$env:USERPROFILE\.codex" | Out-Null
New-Item -ItemType File -Force "$env:USERPROFILE\.codex\auth.json" | Out-Null
New-Item -ItemType File -Force "$env:USERPROFILE\.codex\config.toml" | Out-Null
```

### 第三步：写入 Codex 配置

继续在 PowerShell 中复制并执行：

```powershell
@'
model_provider = "newapi_channel_conn"
model = "gpt-5.5"
model_reasoning_effort = "high"
disable_response_storage = true
preferred_auth_method = "apikey"

[model_providers.newapi_channel_conn]
name = "newapi_channel_conn"
base_url = "https://fullstacklab.xyz/v1"
env_key = "OPENAI_API_KEY"
wire_api = "responses"
stream_idle_timeout_ms = 600000
stream_max_retries = 2
request_max_retries = 2

[windows]
sandbox = "elevated"
'@ | Set-Content -Encoding UTF8 "$env:USERPROFILE\.codex\config.toml"
```

### 第四步：写入环境变量

继续执行下面这条命令。

注意：把 `你自己真实的key` 替换成你在全栈实验室平台后台创建的真实 API Key。

```powershell
[Environment]::SetEnvironmentVariable("OPENAI_API_KEY", "你自己真实的key", "User")
```

示例格式如下，实际使用时不要照抄示例 Key：

```powershell
[Environment]::SetEnvironmentVariable("OPENAI_API_KEY", "sk-xxxxxxxxxxxxxxxx", "User")
```

### 第五步：重启终端和 Codex

环境变量写入后，需要完全关闭当前 PowerShell、Codex 和相关终端窗口，然后重新打开。

### 第六步：下载并打开 Codex 测试

安装或打开 Codex 后，输入一个简单测试提示词：

```text
请回复“配置已生效”。
```

如果能正常回复，说明 Codex 已经成功使用全栈实验室接口。

如果提示 `401 Unauthorized`，通常是 API Key 错误或环境变量没有生效。可以重新复制平台后台的 Key，再执行一次环境变量命令，并重启 PowerShell。


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
