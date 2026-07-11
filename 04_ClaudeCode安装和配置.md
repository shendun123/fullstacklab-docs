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
https://fullstacklab.xyz
```

导入完成后，完全关闭 Claude Code 和相关终端，再重新打开。可以发送测试消息：

```text
请回复“配置已生效”。
```

如果能收到正常回复，说明配置已生效。

## CC Switch 桌面端手动配置

如果后台的“一键导入到 CCS”按钮无法使用，也可以在 CC Switch 桌面软件中手动填写 API URL 和 API Key。Windows、macOS 和 Linux 的界面操作基本一致。

### 第一步：关闭正在运行的 Claude Code

配置前，先退出当前 Claude Code 会话：

```text
按 Ctrl + C 退出 Claude Code
```

然后关闭所有正在运行 Claude Code 的 PowerShell、命令提示符、终端或 VS Code 终端窗口，避免旧配置仍被占用。

### 第二步：打开 CC Switch 并选择 Claude Code

打开已经安装好的 CC Switch 桌面软件。

在软件顶部或左侧的应用列表中选择：

```text
Claude Code
```
或
```text
Claude Desktop
```
注意：使用桌面软件就选 `Claude Desktop`，使用命令行就选`Claude Code `，两者配置文件不同。

### 第三步：新增自定义供应商

在 Claude Code 供应商页面中：

1. 点击右上角的 `+` 按钮或“添加供应商”。
2. 选择“应用专属供应商”。
3. 在预设列表中选择“自定义”。
4. 进入自定义供应商配置页面。

不同版本的 CC Switch 按钮名称可能略有差异，例如显示为“新增供应商”“自定义配置”或“Custom”，以软件实际界面为准。

### 第四步：填写 API URL 和 API Key

按照下面的信息填写：

| 配置项 | 填写内容 |
| --- | --- |
| 供应商名称 | `全栈实验室` |
| API Key | 在全栈实验室后台创建的真实 API Key |
| API 端点 / Endpoint URL / Base URL | `https://fullstacklab.xyz` |
| API 类型或协议（如果有） | 选择 `Anthropic`、`Claude` 或 `Anthropic Messages API` |
| 备注（可选） | `全栈实验室 Claude Code` |

API URL 请填写：

```text
https://fullstacklab.xyz
```

Claude Code 配置通常不需要在地址后面添加 `/v1`，也不要填写成具体的 `/v1/messages` 接口路径。

API Key 示例格式如下，实际使用时请替换成自己后台创建的真实 Key：

```text
sk-xxxxxxxxxxxxxxxx
```

不要把 API Key 发到群聊、工单截图、公开文档或代码仓库中。

### 第五步：检查自定义 JSON 配置

部分 CC Switch 版本会在页面下方显示 JSON 配置编辑框。配置内容可参考：

```json
{
  "env": {
    "ANTHROPIC_API_KEY": "sk-xxxxxxxxxxxxxxxx",
    "ANTHROPIC_BASE_URL": "https://fullstacklab.xyz",
    "CLAUDE_AUTOCOMPACT_PCT_OVERRIDE": "78"
  }
}
```

需要把示例中的 `sk-xxxxxxxxxxxxxxxx` 替换成真实 API Key。

如果页面已经通过单独输入框填写了 API Key 和 Endpoint URL，CC Switch 通常会自动生成对应 JSON，无需重复修改。保存前重点检查：

- `ANTHROPIC_API_KEY` 是否为完整、有效的 API Key
- `ANTHROPIC_BASE_URL` 是否为 `https://fullstacklab.xyz`
- JSON 是否存在多余逗号、缺少引号或括号不完整

如果界面出现“需要本地路由映射”“OpenAI Chat Completions 路由”或类似开关，使用全栈实验室 Claude Code 接口时通常保持关闭，并使用 Anthropic/Claude 协议直连。

### 第六步：保存并切换到该供应商

填写完成后：

1. 点击“添加”或“保存”。
2. 返回 Claude Code 供应商列表。
3. 找到刚创建的“全栈实验室”供应商卡片。
4. 点击“启用”“切换”或供应商卡片本身。
5. 确认该卡片显示为“当前使用”“已启用”或高亮状态。

只保存供应商但没有切换为当前供应商时，Claude Code 可能仍然使用之前的配置。

CC Switch 会把当前供应商配置写入 Claude Code 的配置文件：

```text
Windows: %USERPROFILE%\.claude\settings.json
macOS/Linux: ~/.claude/settings.json
```

### 第七步：彻底重启 Claude Code

配置完成后，必须彻底重启 Claude Code，不能只在原会话中继续使用。

#### Windows

1. 在 Claude Code 窗口中按 `Ctrl + C` 退出。
2. 关闭所有 PowerShell、CMD 和 VS Code 终端窗口。
3. 重新打开一个新的 PowerShell 窗口。
4. 执行：

```powershell
claude
```

#### macOS/Linux

1. 在 Claude Code 窗口中按 `Control + C` 退出。
2. 关闭当前 Terminal、iTerm2 或 VS Code 终端窗口。
3. 重新打开一个新的终端。
4. 执行：

```bash
claude
```

如果 Claude Code 仍然读取旧配置，可以先完全退出 CC Switch 和 Claude Code，再重新打开 CC Switch，确认“全栈实验室”仍是当前供应商，然后重新启动 Claude Code。

### 第八步：测试配置是否可用

进入 Claude Code 后，先发送一个最简单的测试提示词：

```text
请只回复：全栈实验室 Claude Code 配置成功
```

能够正常回复，说明 API URL、API Key 和基础调用已经生效。

接着可以在一个测试目录中发送：

```text
请列出当前目录中的文件，不要创建、删除或修改任何文件。
```

如果 Claude Code 能正常读取并列出当前目录，说明基础对话和工具调用都可以使用。

### 第九步：检查实际写入的配置

如果配置后仍不能使用，可以检查 CC Switch 是否已经正确写入配置文件。

Windows PowerShell：

```powershell
Get-Content "$env:USERPROFILE\.claude\settings.json"
```

macOS/Linux：

```bash
cat ~/.claude/settings.json
```

应当能看到类似内容：

```json
{
  "env": {
    "ANTHROPIC_API_KEY": "sk-xxxxxxxxxxxxxxxx",
    "ANTHROPIC_BASE_URL": "https://fullstacklab.xyz"
  }
}
```

检查时不要把包含完整 API Key 的截图或配置文件发送给其他人。需要提供截图排查时，应先把 Key 中间部分打码。

### CC Switch 桌面配置常见问题

| 现象 | 可能原因和处理方式 |
| --- | --- |
| 保存后 Claude Code 仍走原来的接口 | 只保存但没有切换供应商；回到供应商列表，将“全栈实验室”设为当前供应商 |
| 修改配置后没有变化 | Claude Code 旧进程仍在运行；按 `Ctrl + C` 退出并关闭全部终端后重新打开 |
| 提示 `401 Unauthorized` | API Key 填写错误、复制不完整、Key 已失效，或账户余额/权限异常；回后台重新复制可用 Key |
| 提示 `404 Not Found` | Base URL 填写成了错误接口路径；Claude Code 中应使用 `https://fullstacklab.xyz`，不要手动追加 `/v1/messages` |
| 提示连接失败或超时 | 检查网络、域名访问、系统代理、防火墙和 CC Switch 中的 Endpoint URL |
| 打开后要求登录 Anthropic 官方账号 | 当前供应商没有成功切换，或仍在使用 Claude 官方配置；回 CC Switch 重新选择“全栈实验室” |
| `settings.json` 中没有新配置 | CC Switch 没有写入权限、选择了 Claude Desktop 而不是 Claude Code，或保存时失败 |
| JSON 配置无法保存 | 检查双引号、逗号和大括号，JSON 最后一项后面不能有多余逗号 |
| Key 输入后再次打开看不到完整内容 | CC Switch 通常会自动隐藏密钥，这是正常现象；可点击输入框旁的眼睛图标临时查看 |

## 不使用 CC Switch 时手动修改配置文件

如果需要手动配置，准备：

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
| 连接失败 | 检查 Base URL 是否为 `https://fullstacklab.xyz` |
| JSON 格式错误 | 检查 `settings.json` 是否有多余逗号、缺少引号或括号不匹配 |
