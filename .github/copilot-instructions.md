# AI Agent Instructions

## Project Overview

This is a **Marp presentation project** about Agent Skills education, not a traditional codebase. The primary artifact is [slides.md](../slides.md) — a Marp-formatted presentation deck targeting technical developers learning about Agent Skills.

**Key deliverables**: Marp slides (markdown) → PDF export for conferences/workshops.

## Critical File Relationships

```
slides.md           ← Main presentation (strict format rules)
  └─ references → docs/research-*.md (source citations)
CLAUDE.md           ← Comprehensive editorial guidelines
docs/
  ├─ research-skills-security.md        ← CVEs, attack vectors
  ├─ research-vscode-agent-skills.md    ← VS Code implementation
  └─ research-org-restructuring.md      ← Organizational transformation content
```

## Marp Format Requirements (Non-Negotiable)

[slides.md](../slides.md) has **strict technical constraints** to prevent visual overflow:

- **Frontmatter**: Must maintain `marp: true` and style block
- **Slide separator**: `---` only (three hyphens)
- **Canvas**: 16:9 (1280×720) — content exceeding this is invisible
- **Max content per slide**:
  - 1 table + 1 code block + 1 blockquote, OR
  - 2 small code blocks
  - Tables ≤6 rows (including header)
  - Code blocks ≤10 lines
- **Style variables**: `#e53e3e` (red, emphasis), `#2b6cb0` (blue, code/links), `#ffffff` (background)

**Common mistakes to avoid**:
- ❌ Adding too many bullet points (test in Marp preview)
- ❌ Long tables/code blocks → split across multiple slides
- ❌ Forgetting to close YAML frontmatter with `---`

## Content Guidelines

### 1. Factual Accuracy & Citations
Every factual claim **must** cite sources in blockquote format at slide bottom:

```markdown
> Source: [Name](URL)
> Sources: [Name1](URL1) · [Name2](URL2)
```

Research files in [docs/](../docs/) contain vetted sources — cross-reference before adding new claims.

### 2. Presentation Narrative Structure
Slides follow a **pedagogical progression** (audience: devs unfamiliar with Skills):

1. Evolution context (Prompt → MCP → Skills → Agent)
2. Skills definition (open standard by Anthropic)
3. Individual productivity value
4. **Team/company value** (key differentiator: knowledge codification)
5. Skills vs MCP comparison
6. VS Code hands-on demo
7. Security risks (with real CVEs)
8. Summary + Q&A with references

### 3. Language & Tone
- **English** for all content (slides, code comments, alt text)
- Accessible but technical — no condescension, no jargon without context
- Use concrete examples from [docs/research-vscode-agent-skills.md](../docs/research-vscode-agent-skills.md)

### 4. Key Messaging (Per [CLAUDE.md](../CLAUDE.md))
- Skills = open standard by Anthropic, adopted across 25+ tools
- Frame as **team knowledge management**, not just personal productivity
- **Emphasize organizational transformation** — role evolution (Tech Leads → architects), cross-team collaboration (breaking silos), workflow reinvention (AI handles checklists, humans focus on strategy)
- **Skills as team assets** — versioned, reviewable, executable knowledge that survives turnover
- Highlight Skill Creator as the bootstrap tool ("a Skill that writes Skills")
- Security section: use plain English + real CVEs (e.g., CVE-2025-54794)

## Development Workflows

### Build & Verify
```bash
mvn verify  # Validate Marp syntax + generate PDF
mvn test    # Run test suite (if any)
```

### Preview Changes
Open [slides.md](../slides.md) in VS Code → Marp extension auto-previews.  
**Always preview** after edits to check for overflow.

## File-Specific Conventions

### [CLAUDE.md](../CLAUDE.md) (Editorial Bible)
Contains comprehensive rules:
- Presentation scope & speaking points
- Slide quality standards (overflow checks)
- Citation format
- Company Skills adoption roadmap details

**When editing slides**: re-read relevant sections in [CLAUDE.md](../CLAUDE.md) first.

### [docs/research-*.md](../docs/)
- [research-skills-security.md](../docs/research-skills-security.md): CVEs, attack vectors, defense strategies
- [research-vscode-agent-skills.md](../docs/research-vscode-agent-skills.md): VS Code implementation details, three-level loading
- [research-org-restructuring.md](../docs/research-org-restructuring.md): Organizational transformation content — role evolution, cross-team collaboration, workflow reinvention (emphasizes Skills as team assets)

Use these as **authoritative sources** for claims in slides.

## Project-Specific Patterns

### Pattern: Slide Overflow Prevention
Before adding content to a slide:
1. Count existing elements (tables? code blocks? bullet points?)
2. If >2 heavy elements, create new slide with `---`
3. Preview in Marp to confirm no cutoff

### Pattern: Citation Workflow
1. Make a claim in slides
2. Search [docs/](../docs/) for supporting source
3. Add blockquote citation at slide bottom
4. Verify URL is reachable

### Pattern: Code Examples in Slides
Keep YAML examples realistic but **minimal**:
```yaml
---
name: code-review
description: "Review code for bugs and style"
---
```
Full examples live in research docs, not slides.

## Common Pitfalls

1. **Adding slides without checking [CLAUDE.md](../CLAUDE.md) guidelines** → inconsistent messaging
2. **Copying verbose examples from docs/** → overflow on 16:9 canvas
3. **Missing source citations** → violates factual accuracy requirement
4. **Breaking narrative flow** → slides are ordered pedagogically, not topically
5. not generate UML  c

## Where Things Are

| Need | Location |
|------|----------|
| Main content | [slides.md](../slides.md) |
| Editorial rules | [CLAUDE.md](../CLAUDE.md) |
| Security research | [docs/research-skills-security.md](../docs/research-skills-security.md) |
| VS Code impl details | [docs/research-vscode-agent-skills.md](../docs/research-vscode-agent-skills.md) |
| Org transformation | [docs/research-org-restructuring.md](../docs/research-org-restructuring.md) |
| Build commands | Workspace tasks: `verify`, `test` |

## Quick Start for AI Agents

1. **Read** [CLAUDE.md](../CLAUDE.md) for editorial guidelines
2. **Preview** [slides.md](../slides.md) to understand current state
3. **Consult** [docs/](../docs/) before making factual claims
4. **Test** in Marp preview before committing changes
5. **Verify** with `mvn verify` before declaring done

---

*Last updated*: 2026-02-03  
*For questions about Agent Skills standard*: https://agentskills.io
