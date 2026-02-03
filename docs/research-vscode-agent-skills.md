# VS Code Agent Skills 研究文档

> 研究日期：2026-02-02

## 1. 什么是 VS Code Agent Skills

Agent Skills 是 VS Code 1.108+（2025 年 12 月）中引入的实验性功能，允许 GitHub Copilot（以及 Claude Code）根据上下文自动加载专门的指令包。

Skills 遵循 [Agent Skills 开放标准](https://agentskills.io)（由 Anthropic 发起），是跨工具的——同一个 Skill 可以在 VS Code Copilot、Claude Code CLI、Codex CLI 中使用。

**核心定义**：Skills = 文件夹（SKILL.md + 脚本 + 资源），AI Agent 根据需要自动发现和加载。

## 2. 目录结构

### 项目级 Skills（团队共享，提交到 Git）
```
.github/skills/       ← 推荐（开放标准）
.claude/skills/       ← 兼容（Anthropic 传统路径）
```

### 个人级 Skills（只在自己机器上生效）
```
~/.copilot/skills/    ← 推荐（开放标准）
~/.claude/skills/     ← 兼容（Anthropic 传统路径）
```

### 单个 Skill 的文件结构
```
.github/skills/my-review/
├── SKILL.md              ← 必需：元数据 + 指令
├── checklist.md          ← 可选：支持文件
├── scripts/
│   └── run-lint.sh       ← 可选：脚本
└── examples/
    └── good-review.md    ← 可选：示例
```

## 3. SKILL.md 格式

```yaml
---
name: my-review                                    # 必需，小写+连字符，最长 64 字符
description: "Review code for bugs and style"      # 必需，最长 1024 字符
---

# 代码审查指令

## 步骤
1. 读取变更文件
2. 检查安全漏洞
3. 检查代码风格
4. 输出结构化反馈
```

**关键字段**：
- `name`：成为 `/slash-command` 的名称（如 `/my-review`）
- `description`：Agent 用它来判断何时自动加载该 Skill

## 4. 加载机制：三级渐进式披露（Progressive Disclosure）

| 级别 | 加载内容 | 何时加载 |
|------|---------|---------|
| **Level 1** | 元数据（name + description） | 始终可用，用于判断相关性 |
| **Level 2** | SKILL.md 完整内容 | 当用户请求匹配 description 时 |
| **Level 3** | 支持文件（脚本、示例等） | 当指令中引用时按需加载 |

这意味着 Skill 不会无谓消耗 Token——只有被触发时才加载完整内容。

## 5. 如何在 VS Code 中启用

### 前置条件
- VS Code 1.108+ 或 VS Code Insiders
- GitHub Copilot 扩展已安装（如使用 Copilot Agent Skills）
- 或 Claude Code 扩展已安装（如使用 Claude Skills）

### 启用 Agent Skills（Copilot）
1. 打开 VS Code Settings（`Cmd+,`）
2. 搜索 `chat.useAgentSkills`
3. 勾选 **Use Agent Skills**

### Claude Code 方式
Claude Code 扩展默认支持 Skills，无需额外设置。只要 `.claude/skills/` 目录存在即可。

## 6. 如何测试 Skill 已正确加载

### 方法一：直接调用 Slash Command
```
在 Claude Code / Copilot Chat 中输入：
/my-review
```
如果 Skill 正确加载，Agent 会按照 SKILL.md 中的指令执行。

### 方法二：使用 `/context` 检查（Claude Code）
```
输入 /context 查看当前加载的 Skills 列表和 Token 预算使用情况
```
如果你的 Skill 出现在列表中，说明已被发现。如果有 "excluded" 警告，说明 Token 预算已满。

### 方法三：间接触发测试
发送一个与 Skill description 匹配的请求，观察 Agent 是否自动加载该 Skill：
```
"请帮我审查这个 PR 的代码质量"
→ 如果 my-review skill 的 description 匹配，应自动触发
```

### 方法四：格式验证检查清单
- [ ] `SKILL.md` 位于正确路径（`.github/skills/xxx/SKILL.md` 或 `.claude/skills/xxx/SKILL.md`）
- [ ] 文件以 `---` 开头，YAML frontmatter 格式正确
- [ ] `name` 和 `description` 字段存在且为顶层标量
- [ ] 指令部分简洁、步骤清晰
- [ ] 引用的脚本和资源文件实际存在

## 7. 常见问题排查

| 问题 | 原因 | 解决方案 |
|------|------|---------|
| Skill 不触发 | description 不够明确 | 优化 description，明确写出"何时使用" |
| YAML 解析失败 | frontmatter 格式错误 | 确保 `---` 分隔符正确，字段为顶层标量 |
| Token 预算超限 | Skills 太多或描述太长 | 用 `/context` 检查，精简描述，或设置 `SLASH_COMMAND_TOOL_CHAR_BUDGET` 环境变量 |
| 自动触发不稳定 | Agent 判断不够精准 | 在 `CLAUDE.md` 中列出 Skills 清单作为辅助（workaround） |

## 8. 已知限制

- Agent Skills 在 VS Code Copilot 中仍为 **Preview** 状态
- 自动触发（agent_invocable）不如手动调用可靠，有时需要在 prompt 中明确提及 Skill 名称
- 多个 Skill 的 description 过长时可能超出 Token 预算（默认 15,000 字符）

## 参考资料

- [VS Code 官方文档 - Agent Skills](https://code.visualstudio.com/docs/copilot/customization/agent-skills)
- [Claude Code 官方文档 - Skills](https://code.claude.com/docs/en/skills)
- [Cloud Native Deep Dive - Using Agent Skills in VS Code](https://www.cloudnativedeepdive.com/using-agent-skills-in-vs-code/)
- [Visual Studio Magazine - Hands On with Agent Skills](https://visualstudiomagazine.com/articles/2026/01/11/hand-on-with-new-github-copilot-agent-skills-in-vs-code.aspx)
- [Agent Skills 开放标准](https://agentskills.io)
- [awesome-agent-skills](https://github.com/heilcheng/awesome-agent-skills)
