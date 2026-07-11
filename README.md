<div class="doc-hero">
  <div class="badge">全栈实验室 · 用户操作指南</div>
  <h1>全栈实验室操作文档</h1>
  <p>
    本文档用于帮助用户快速完成购买、工具配置、Claude Code、Codex、Cherry Studio、接口调用以及常见问题排查。
    左侧目录已拆分为工具配置、接口文档和常见问题，方便快速定位。
  </p>
</div>

<div class="doc-grid">
  <div class="doc-card">
    <div class="num">01</div>
    <h3>工具配置</h3>
    <p>完成 Cherry Studio、Claude Code、Codex 等工具配置，Claude Code 页面已包含 ccswitch 一键配置说明。</p>
  </div>

  <div class="doc-card">
    <div class="num">02</div>
    <h3>接口文档</h3>
    <p>按 Chat、Responses、Anthropic、Gemini、图像、视频等接口拆分查看。</p>
  </div>

  <div class="doc-card">
    <div class="num">03</div>
    <h3>常见问题</h3>
    <p>集中查看配置不生效、鉴权失败、连接失败、任务超时等问题。</p>
  </div>
</div>

<div class="doc-panel">

<p><strong>推荐阅读顺序</strong></p>

1. [购买需知](01_购买需知.md)
2. [Cherry Studio 教程](03_CherryStudio教程.md)
3. [Claude Code 安装和配置](04_ClaudeCode安装和配置.md)
4. [Codex 安装和配置](05_Codex安装和配置.md)
5. [接口文档](10_接口文档.md)
6. [常见问题](09_常见问题.md)


</div>

<div class="doc-panel">

<p><strong>统一 API 地址</strong></p>

```text
https://fullstacklab.xyz
```

OpenAI 兼容协议一般使用：

```text
https://fullstacklab.xyz/v1
```

</div>

<div class="doc-panel">

<p><strong>购买需知</strong></p>

在开始使用前，先确认自己的使用场景：只接入 Claude Code、只接入 Codex，还是两个工具都需要。购买、充值、额度开通和活动规则应以 全栈实验室 后台的实时显示为准。

首次使用建议先小额充值或开通基础额度，确认 API Key、Base URL 和目标工具都能正常工作后，再根据实际消耗追加额度。

全栈实验室 的余额属于站内额度，不等同于 Claude 官方账户余额，也不等同于 OpenAI 官方账户余额。通过 全栈实验室 调用 Claude Code、Codex 或其他客户端时，费用会从 全栈实验室 账户额度里扣除。

倍率可以理解为不同模型或不同服务通道的计费系数。倍率可能随模型、通道、市场和服务状态变化，所以不应把某个倍率当成永久固定值。实际扣费以后台记录为准。

配置相关工具时，Base URL 使用：

```text
https://fullstacklab.xyz
```

如果连接失败，优先检查 Base URL 是否写错，末尾是否误加了多余字符，以及 API Key 是否复制完整。

技术支持微信：fengwy2012

</div>
