# Agent Skills Presentation

## 项目说明
这是一个 Marp 演示文稿项目，主题为"从 Prompt Engineering 到 AI Agent — Agent Skills 实战指南"。

## 文件结构
- `slides.md` — Marp 格式的演示文稿主文件
- `docs/` — 研究文档
  - `research-skills-security.md` — Skills & MCP 安全风险研究
  - `research-vscode-agent-skills.md` — VS Code Agent Skills 研究

## 编辑规范
- `slides.md` 必须保持 `marp: true` 在 frontmatter 中
- 使用 `---` 分隔幻灯片
- 亮色主题（#ffffff 背景，#e53e3e 强调色，#2b6cb0 代码/链接色）
- 演讲面向有技术背景但不了解 Skills 的开发者，内容需通俗易懂
- 中文为主，技术术语保留英文原文

## 演讲口径
- Skills 标准由 Anthropic 创建并开源（agentskills.io），现已被 GitHub Copilot、OpenAI Codex 等广泛支持
- 演讲开头提及 Anthropic 是 Skills 标准的发起者，之后不再提及 Claude Code
- 所有实战演示和操作步骤均以 **VS Code Agent Skills**（GitHub Copilot）为主线
- 不涉及 Claude Code 扩展安装、`/plugins` 等 Anthropic 特有功能

## 演讲结构
PPT 按听众理解顺序组织，开头有可点击跳转的大纲页：

1. **演进全景** — 背景铺垫，一页表格概览 Prompt → MCP → Skills → Agent
2. **什么是 Agent Skills** — 定义、由 Anthropic 开源、开放标准
3. **Skills 对个人的价值** — 个人效率提升，知识复用，减少重复劳动
4. **Skills 对团队/公司的价值** — 标准化最佳实践、知识沉淀、新人 onboarding、跨团队复用
5. **Skills vs MCP** — 认知层 vs 基础设施层，互补关系
6. **VS Code 实战** — 启用、创建、加载机制、验证
7. **安全风险** — 通俗讲解 + 例子
8. **总结 & Q&A**

## 引用规范
- PPT 中每个事实性声明（factual claim）都应在幻灯片底部以 blockquote 格式附上来源引用
- 单来源格式：`> Source: [名称](URL)`
- 多来源格式：`> Sources: [名称1](URL1) · [名称2](URL2)`
- Q&A 页的 References 列表应包含所有主要来源的完整链接

## 幻灯片质量规范
- **语言**：所有幻灯片内容必须使用英文（包括表格、代码注释、blockquote 等）
- **溢出检查**：每页幻灯片不得超出 Marp 16:9 (1280×720) 的可视区域
  - 单页最多：1 个表格 + 1 个代码块 + 1 行 blockquote，或 2 个小代码块
  - 表格行数建议不超过 6 行（含表头）
  - 代码块行数建议不超过 10 行
  - 如内容过多，应拆分为多页

## 内容要点
- Skills 不只是个人工具，更是团队知识管理的载体
- 每个团队/公司应该创建自己的 Skills，原因：
  - 编码团队特有的代码规范和审查标准
  - 沉淀 tribal knowledge，避免关键人离职导致知识流失
  - 新人入职即可获得团队"老手"的工作流
  - 跨团队共享最佳实践（如安全审查、部署流程）
  - 可版本控制、可 code review、可持续迭代
