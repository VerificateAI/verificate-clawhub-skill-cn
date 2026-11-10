---
name: verificate-cn
description: "OpenClaw 的信任层。在信任之前，先验证 AI 编写的代码、工具调用与研究答复——确定性否决检查 + Claude Haiku 5.5 模型评审，拥有一票否决权。免费试用，无需注册。"
homepage: https://verificate.ai/mcp
metadata:
  {
    "openclaw":
      {
        "emoji": "🛡️",
        "requires": {},
        "mcp":
          {
            "verificate":
              {
                "type": "http",
                "url": "https://mcp.verificate.ai/mcp",
              },
          },
      },
  }
---

# verificate（中文）

**OpenClaw 的信任层。** ClawHub 技能功能强大，但并不可信——安全审计（Snyk）反复在社区技能中发现提示注入、恶意软件与凭证窃取，而每一条 AI 答复无论对错都显得信心十足。Verificate 就是那个用来验证其他技能的技能。

它把 OpenClaw 接入托管的 Verificate MCP 服务器（无需注册——每台机器可免费验证 100 次）：

- **`validate_ai_output`** — AI 编写代码**及文档**的合入闸门：先运行确定性否决检查（模拟/占位符、刷分测试、虚构的标准库 API、日志中的密钥、虚假完成声明），任意一项都可否决且不可被覆盖；随后由 Claude Haiku 5.5 按 ISO/IEC 25010 与 ISO/IEC 5055 评审其余部分（包括第三方 API 调用是否真实）。返回唯一的裁决、按严重程度排序的问题清单与回执。
- **`validate_plan`** — 在编写任何代码之前，先对设计或方案进行验证。
- **`analyze_code`** — 深度顾问分析：性能、可扩展性、故障模式。仅给出评分与结论，不做裁决、不会阻断。
- **`generate_code`** — 一步完成：生成代码并立即验证。

## 快速开始

```
openclaw mcp add verificate --http https://mcp.verificate.ai/mcp
```

然后让 OpenClaw *"在完成之前，用 verificate 验证一下。"* 一条来自生产网关的真实拒绝记录：*"`stripe.Inventory` 并非有效的 Stripe SDK 资源"* ·  *"N+1 同步 API 调用……将触发 Stripe 限流"* ·  *"Stripe API 要求金额为整数分。"* 每一条，若不拦截都是数小时的线上排障。

## 让每一次交互都受到保护（推荐）

把下面这段加入你的 OpenClaw 智能体指令，让它自动自检，而不必每次都手动请求：

```text
在把任何实质性的代码改动、文档或方案当作完成之前，
先调用 validate_ai_output（方案设计用 validate_plan）。
若结果为“拒绝”，修复每一项问题后再次验证——绝不呈交被拒的成果。
把闸门捕获到的任何问题，用一句话告诉我。
```

## 信任与隐私

只读：你的代码只被分析，绝不执行，绝不用于模型训练。本技能唯一的网络请求指向 `https://mcp.verificate.ai/mcp`——没有其他外发流量，无可窃取之物。开源。隐私政策：https://verificate.ai/privacy

免费的 100 次用完后，可开启 30 天试用（无需信用卡，之后 $30/月）：https://verificate.ai/auth/signup

---

English listing: https://github.com/VerificateAI/verificate-clawhub-skill · All clients: https://github.com/VerificateAI/verificate-mcp-quickstart
