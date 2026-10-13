# clean-principles

> Clean coding principles for AI agents — SOLID, Clean Code, DRY/KISS/YAGNI, Design Patterns, Architecture, Testing, Clarify Before Build.
> Language-agnostic. Works with Antigravity (AGY), Claude Code, Cursor, Codex, and any agent supporting [agentskills.io](https://agentskills.io).

## What's Included

### Always-On Rules
Automatically applied in every coding session:

- **`rules/core-principles.md`** — Naming conventions, function rules, design principles baseline
- **`CLAUDE.md`** / **`AGENTS.md`** — Picked up automatically by Claude Code and other agents

### On-Demand Skills
Loaded when relevant — deep-dive reference for each topic:

| Skill | Trigger |
|-------|---------|
| `solid-principles` | Designing classes/modules, extensibility problems |
| `clean-code` | Naming, functions, comments, readability |
| `dry-kiss-yagni` | Seeing duplication, over-engineering, premature abstraction |
| `design-patterns` | Recurring structural/behavioral problems |
| `architecture-principles` | System structure, module boundaries, layer design |
| `testing-principles` | Writing/reviewing tests, brittle/slow tests |
| `clarify-before-build` | Ambiguous scope words ("production-ready", "simple", "quick", "clean") |

---

## ⚖️ Comparison: With vs. Without `clean-principles`

How AI agent output changes when `clean-principles` is active:

| Dimension | ❌ Without Plugin (Default AI Behavior) | ✅ With `clean-principles` |
|---|---|---|
| **Class Architecture** | Superficial cleanup. Leaves "God Classes" with mixed responsibilities (DB queries, email, domain logic all in one place). | Enforces **SRP** & **DIP**. Separates domain models, pure calculators, and infrastructure behind interfaces (`Protocol`). |
| **API Design** | Keeps boolean flag arguments (`process(True)`). Cryptic parameter abbreviations (`u_id`, `d`, `flag`). | Eliminates flag arguments with intention-revealing methods (`processStandard()`, `processPromotional()`). |
| **Ambiguous Requests** | **Silent scope expansion & hedging**: Outputs 1,400+ tokens of speculative hybrid code with warnings. High rework rate (~80%). | **Clarify before build**: Detects ambiguous triggers (*"simple"*, *"production-ready"*), asks 1 spectrum question (~120 tokens), preventing rework. |
| **Error Handling** | Swallows errors (`except: return None`) or returns mixed types (string error messages, `None`, dicts). | Specific custom exceptions, fails fast, no `None` returns for collections, consistent typed domain entities. |
| **Over-Engineering** | Over-abstracts prematurely (creates factories, generic interfaces, and design patterns for 20-line scripts). | Pragmatic guardrails: explicit *"When NOT to apply"* rules (e.g. YAGNI > OCP for single implementations; scripts < 200 lines skip boilerplate). |
| **Testability** | Tight coupling to external services (SMTP, database sockets). Hard to test without heavy mocking or live servers. | Inverted dependencies allow **100% in-memory unit testing** in milliseconds. |

---

## Installation

### 📦 Via npm (recommended — works everywhere)

```bash
npm install clean-principles
```

Then export skills into your agent config:

```bash
# Using npx agentskills (agentskills.io standard)
npx agentskills export clean-principles

# Using skillpm
npx skillpm install clean-principles

# Manual copy from node_modules
cp -r node_modules/clean-principles/skills ~/.agents/skills/
```

---

### 🤖 Antigravity / AGY

**Global (all projects):**
```bash
cp -r clean-principles ~/.gemini/config/plugins/clean-principles
```

Add to `~/.gemini/config/plugins.json`:
```json
{ "plugins": ["~/.gemini/config/plugins/clean-principles"] }
```

**Per-project (share with team via git):**
```bash
cp -r clean-principles your-project/.agents/plugins/clean-principles
```

---

### 🟣 Claude Code

```bash
# Clone into your project
git clone https://github.com/Khuirul-Huda/clean-principles .claude/plugins/clean-principles
```

Or install via npm and reference in `.claude/settings.json`:
```json
{
  "plugins": ["./node_modules/clean-principles"]
}
```

`CLAUDE.md` is automatically picked up by Claude Code when present in the plugin folder.

---

### 🔲 Cursor

```bash
cp -r clean-principles ~/.cursor/plugins/clean-principles
```

Or reference in your project's `.cursorrules` by copying the content of `AGENTS.md`.

---

### 📖 Codex / OpenAI Codex

```bash
cp -r clean-principles ~/.codex/plugins/clean-principles
```

---

### 🌐 Any agent that reads `AGENTS.md`

Copy `AGENTS.md` to your project root — it contains the always-on rules in a format readable by any agent.

```bash
cp clean-principles/AGENTS.md your-project/AGENTS.md
```

---

### 📋 Manual / Minimal install

Copy just the skills you need:
```bash
# All skills
cp -r clean-principles/skills/* ~/.agents/skills/

# Or just specific ones
cp -r clean-principles/skills/solid-principles ~/.agents/skills/
cp -r clean-principles/skills/clean-code ~/.agents/skills/
```

---

## Philosophy

- **Compact, not verbose** — every skill is scannable in 30 seconds
- **Language-agnostic** — principles apply regardless of Python, TypeScript, Go, etc.
- **Good vs Bad examples** — every principle shows what to avoid and what to do instead
- **When principles conflict** — explicit guidance on which wins and when

---

## Structure

```
clean-principles/
├── plugin.json                     # Antigravity/AGY plugin config
├── package.json                    # npm package config
├── CLAUDE.md                       # Auto-loaded by Claude Code
├── AGENTS.md                       # Auto-loaded by Codex & generic agents
├── LICENSE                         # MIT License
├── README.md
├── .claude-plugin/
│   └── plugin.json                 # Claude plugin marketplace manifest
├── .cursor-plugin/
│   └── plugin.json                 # Cursor plugin manifest
├── .codex-plugin/
│   └── plugin.json                 # OpenAI Codex plugin manifest
├── rules/
│   └── core-principles.md          # Always-on rules (AGY)
└── skills/
    ├── solid-principles/SKILL.md
    ├── clean-code/SKILL.md
    ├── dry-kiss-yagni/SKILL.md
    ├── design-patterns/SKILL.md
    ├── architecture-principles/SKILL.md
    ├── testing-principles/SKILL.md
    └── clarify-before-build/SKILL.md
```

---

## Contributing

Skills follow the [agentskills.io specification](https://agentskills.io/specification).

To add a new principle skill:
1. Create `skills/<skill-name>/SKILL.md`
2. Frontmatter: `name` + `description` (start with "Use when...")
3. Keep it scannable — target < 500 words
4. Include bad/good code examples
5. Add a quick-reference table or self-check checklist

## License

MIT
