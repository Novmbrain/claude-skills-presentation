# Claude Skills & MCP 安全风险研究总结

> 研究日期：2026-02-02

## 1. Tool Poisoning（工具投毒）

在 MCP 工具的 description 字段中嵌入隐藏恶意指令。AI 模型能看到完整描述但用户界面通常不显示。当 Agent 连接 MCP Server 时，通过 `tools/list` 获取工具元数据，这些元数据直接进入模型上下文。

**真实案例**：Invariant Labs 利用投毒工具，成功让 Cursor 读取并泄露用户的 `~/.ssh/id_rsa` 和 `mcp.json` 配置文件。

**研究数据**：MCPTox 基准测试中，20 个主流 LLM Agent 对 45 个真实 MCP Server（353 个工具）进行测试，o1-mini 攻击成功率高达 72.8%。越强的模型越容易被攻击，因为它们更好地"遵循指令"。

- 来源：[Invariant Labs](https://invariantlabs.ai/blog/mcp-security-notification-tool-poisoning-attacks)
- 来源：[MCP Manager - Tool Poisoning](https://mcpmanager.ai/blog/tool-poisoning/)

## 2. Prompt Injection（提示注入）

通过文件内容、网页内容或工具返回值注入恶意指令，劫持 Agent 行为。

**CVE-2025-54794 & CVE-2025-54795**：Claude Code 的命令白名单输入清洗不当，攻击者可构造字符串突破命令上下文，执行未授权 shell 命令。已在 v0.2.111 修复。

- 来源：[Cymulate - InversePrompt](https://cymulate.com/blog/cve-2025-547954-54795-claude-inverseprompt/)
- 来源：[GitHub - CVE-2025-54794](https://github.com/AdityaBhatt3010/CVE-2025-54794-Hijacking-Claude-AI-with-a-Prompt-Injection-The-Jailbreak-That-Talked-Back)

## 3. 数据外泄（Data Exfiltration）

利用受信域绕过网络限制上传敏感数据。

**案例**：研究者 Johann Rehberger 发现，Claude Code 的网络设置 "Package managers only" 允许访问 `api.anthropic.com`。攻击者可通过 prompt injection 让 Claude 用 `curl` 调用 Anthropic Files API，将聊天记录等敏感数据上传到攻击者的 API key 下。

**2026 年 1 月**，同一漏洞在 Anthropic Cowork 产品中被再次发现（PromptArmor 报告）。

- 来源：[The Register - Files API exfiltration](https://www.theregister.com/2026/01/15/anthropics_claude_bug_cowork/)
- 来源：[Security Boulevard](https://securityboulevard.com/2026/01/vulnerability-in-anthropics-claude-code-shows-up-in-cowork/)

## 4. Tool Shadowing（工具影子攻击）

多 MCP Server 连接同一客户端时，恶意 Server 注入工具描述来修改 Agent 对其他受信工具的行为，实现跨服务认证劫持。

- 来源：[Elastic Security Labs](https://www.elastic.co/security-labs/mcp-tools-attack-defense-recommendations)

## 5. 供应链攻击

社区 MCP Server 可能包含已知漏洞。

**案例**：2025 年 6 月，Trend Micro 披露 Anthropic 官方 SQLite MCP Server 含经典 SQL 注入漏洞。该 Server 在归档前已被 fork/复制 5000+ 次，漏洞代码可能仍在下游项目中流通。

- 来源：[Practical DevSecOps](https://www.practical-devsecops.com/mcp-security-vulnerabilities/)

## 6. 其他攻击向量

- **Rug Pull**：工具初始合法，获得信任后突然变为恶意
- **MCP Preference Manipulation (MPMA)**：篡改 Agent 的工具优先级排序
- **Sampling 攻击**：Palo Alto Unit 42 发现的资源盗用、对话劫持、未授权工作负载

## 7. 防御建议

| 层面 | 措施 |
|------|------|
| **权限** | 最小权限原则，敏感操作需手动确认 |
| **来源** | 只使用受信 MCP Server，维护工具注册表 |
| **审查** | 检查工具描述是否含隐藏指令 |
| **隔离** | 沙箱运行，限制网络出站 |
| **监控** | 持续监控工具行为变化 |
| **网关** | 使用 MCP Gateway 做运行时拦截和清洗 |

## 8. 学术参考

- [arXiv: Prompt Injection Attacks on Agentic Coding Assistants](https://arxiv.org/html/2601.17548v1) — 综合 78 篇研究（2021-2026），首次系统分析 skill 架构漏洞，自适应攻击成功率 >85%
- [MCP Official Security Best Practices](https://modelcontextprotocol.io/specification/draft/basic/security_best_practices)
- [Microsoft - Protecting against indirect injection attacks in MCP](https://developer.microsoft.com/blog/protecting-against-indirect-injection-attacks-mcp)
- [Palo Alto Unit 42 - MCP Sampling Attack Vectors](https://unit42.paloaltonetworks.com/model-context-protocol-attack-vectors/)
