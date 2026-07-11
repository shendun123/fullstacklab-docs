# Codex 安装和配置

本节适合想安装 Codex，并通过 全栈实验室 接入 Codex 的用户。

相关入口：

- Codex App 页面：`https://openai.com/codex/for-work/`
- Codex 介绍页：`https://openai.com/index/introducing-the-codex-app/`
- Codex 文档：`https://developers.openai.com/codex`
- Codex npm 包：`https://www.npmjs.com/package/@openai/codex`
- Node.js：`https://nodejs.org/en/download`

如果使用 npm 安装 Codex CLI，需要先安装 Node.js 和 npm。先检查：

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
- Base URL：`https://fullstacklab.xyz`
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
base_url = "https://fullstacklab.xyz"
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


## macOS 版本具体配置步骤

下面是 macOS 用户的手动配置方式，适用于系统默认的 zsh 终端。macOS 版使用与 Windows 相同的模型、Base URL 和环境变量，但不需要 `[windows]` 配置块。

### 第一步：打开终端

打开：

```text
访达 → 应用程序 → 实用工具 → 终端
```

也可以按 `Command + 空格`，搜索“终端”后打开。

### 第二步：安装或检查 Codex CLI

macOS 可以使用官方独立安装脚本：

```bash
curl -fsSL https://chatgpt.com/codex/install.sh | sh
```

也可以继续使用 npm 安装：

```bash
npm install -g @openai/codex
```

安装完成后检查版本：

```bash
codex --version
```

如果提示 `command not found: codex`，请完全关闭终端后重新打开，再执行一次 `codex --version`。

### 第三步：创建 Codex 配置目录和文件

在终端中逐步执行：

```bash
mkdir -p "$HOME/.codex"
touch "$HOME/.codex/auth.json"
touch "$HOME/.codex/config.toml"
```

如果之前已经配置过 Codex，建议先备份旧配置：

```bash
cp "$HOME/.codex/config.toml" "$HOME/.codex/config.toml.bak" 2>/dev/null || true
```

### 第四步：写入 Codex 配置

复制并执行下面整段命令：

```bash
cat > "$HOME/.codex/config.toml" <<'EOF'
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
EOF
```

检查配置文件是否已写入：

```bash
cat "$HOME/.codex/config.toml"
```

### 第五步：写入 API Key 环境变量

执行下面这组命令。终端会提示输入 API Key，输入过程中不会显示字符，输入完成后按回车。

```bash
read -s "OPENAI_API_KEY?请输入全栈实验室 API Key："
echo
export OPENAI_API_KEY
touch "$HOME/.zshrc"
sed -i '' '/^export OPENAI_API_KEY=/d' "$HOME/.zshrc"
printf 'export OPENAI_API_KEY=%q\n' "$OPENAI_API_KEY" >> "$HOME/.zshrc"
source "$HOME/.zshrc"
```

检查环境变量是否已经生效：

```bash
if [ -n "$OPENAI_API_KEY" ]; then echo "OPENAI_API_KEY 已生效"; else echo "OPENAI_API_KEY 未生效"; fi
```

为避免泄露，不要使用 `echo $OPENAI_API_KEY` 输出完整 Key，也不要把 Key 写入项目文件、截图或公开聊天记录。

> 如果你的 macOS 使用 bash，而不是默认的 zsh，请把上面命令中的 `~/.zshrc` 改为 `~/.bash_profile`。

### 第六步：重新打开终端并测试

完全关闭当前终端和 Codex，然后重新打开终端，执行：

```bash
codex --version
codex
```

进入 Codex 后发送：

```text
请回复“配置已生效”。
```

如果能正常回复，说明 Codex 已经通过全栈实验室接口运行。

如果提示 `401 Unauthorized`，请重点检查：

- API Key 是否复制完整；
- `OPENAI_API_KEY` 环境变量是否生效；
- `base_url` 是否为 `https://fullstacklab.xyz/v1`；
- 修改配置后是否完全关闭并重新打开终端。


## Windows 与 macOS 通用验证

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
