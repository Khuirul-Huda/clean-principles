# clean-principles

> Clean coding principles for AI agents — SOLID, Clean Code, DRY/KISS/YAGNI, Design Patterns, Architecture, Testing, Clarify Before Build.
> Language-agnostic. Works with Antigravity (AGY), Claude Code, Codex, GitHub Copilot, Cursor, and any agent supporting [agentskills.io](https://agentskills.io).

---

## ⚡ Quick Install

### Via Marketplace (Recommended)

| Agent | Command |
|---|---|
| **Claude Code** | `/plugin marketplace add Khuirul-Huda/clean-principles`<br>`/plugin install clean-principles@clean-principles` |
| **Antigravity (AGY)** | `agy plugin install https://github.com/Khuirul-Huda/clean-principles` |
| **OpenAI Codex** | `codex plugin marketplace add Khuirul-Huda/clean-principles`<br>`codex plugin add clean-principles@clean-principles` |
| **GitHub Copilot CLI** | `copilot plugin marketplace add Khuirul-Huda/clean-principles`<br>`copilot plugin install clean-principles@clean-principles` |
| **npm / agentskills** | `npx agentskills export clean-principles` |

### Generic / Project Install

For any agent that reads `AGENTS.md` (Cursor, Windsurf, Aider, etc.), copy `AGENTS.md` directly into your project root:

```bash
cp clean-principles/AGENTS.md your-project/AGENTS.md
```

---

## 📦 What's Included

### Always-On Rules
Loaded automatically every session:
- **`rules/core-principles.md`** — Naming, function limits, error handling, clean defaults.
- **`AGENTS.md`** / **`CLAUDE.md`** — Standard project rules picked up across all major agents.

### On-Demand Skills
Loaded dynamically when relevant to the task:

| Skill | Trigger / When to Apply |
|---|---|
| `solid-principles` | Designing classes/modules, interfaces, coupling issues |
| `clean-code` | Naming, small functions, comments, readability |
| `dry-kiss-yagni` | Seeing duplication, over-engineering, premature abstraction |
| `design-patterns` | Structural/behavioral problems (Factory, Strategy, Observer, Repository) |
| `architecture-principles` | System boundaries, layered & hexagonal architecture, high cohesion |
| `testing-principles` | Unit test design, FIRST, AAA pattern, test doubles |
| `clarify-before-build` | Ambiguous requirements ("production-ready", "simple", "quick", "clean") |

---

## ⚖️ Comparison: With vs. Without `clean-principles`

| Dimension | ❌ Without Plugin (Default AI Behavior) | ✅ With `clean-principles` |
|---|---|---|
| **Class & File Sizing** | God classes & 800-line monolithic files mixing DB, UI, and business logic. | Strict **SRP** & **DIP**. Target **≤ 300 lines/file**; cohesive, single-purpose modules. |
| **Readability & Control Flow** | Deeply nested `if/else` staircases (Arrow Anti-Pattern); dense clever one-liners. | **Senior 10+ YoE standard**: Guard clauses, flat indentation (≤ 2 levels), self-documenting code. |
| **Language & Idioms** | Writes "Java in Python" or procedural code in Laravel; ignores linter configs. | **Idiom-first & Toolchain-aware**: Adheres to PEP 8, PSR-12, Effective Go, and project configs. |
| **API Design** | Boolean flags (`process(True)`), cryptic parameter names (`u_id`, `flag`). | Eliminates flags via intention-revealing methods (`processStandard()`, `processPromotional()`). |
| **Ambiguous Requests** | Speculative code expansion; outputs 1,400+ tokens of hybrid guesswork. | **Clarify before build**: Asks 1 spectrum question (~120 tokens), preventing rework. |
| **Error Handling** | Swallows errors (`except: return None`) or returns mixed types. | Specific custom exceptions, fails fast, empty collections instead of `null`/`None`. |
| **Over-Engineering** | Generates premature abstractions and factories for 20-line scripts. | Pragmatic guardrails: explicit *"When NOT to apply"* rules (KISS > patterns for small scripts). |
| **Testability** | Tight coupling to external services; hard to test without mocks. | Inverted dependencies allow fast, in-memory unit testing. |

---

## 📂 Repository Structure

```text
clean-principles/
├── plugin.json                     # Antigravity (AGY) plugin manifest
├── package.json                    # npm / agentskills manifest
├── AGENTS.md                       # Always-on rules for AI agents
├── CLAUDE.md                       # Claude Code entry point
├── .claude-plugin/                 # Claude Code marketplace & plugin manifests
├── .agents/                        # Antigravity & Codex marketplace manifests
├── .github/                        # Copilot CLI marketplace manifest
├── .cursor-plugin/                 # Cursor plugin manifest
├── .codex-plugin/                  # OpenAI Codex plugin manifest
├── rules/                          # Always-on baseline rules
└── skills/                         # 7 on-demand principle skills
```

---

## 🤝 Contributing

Skills follow the [agentskills.io specification](https://agentskills.io/specification). Commit messages follow [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/) (see [`CONTRIBUTING.md`](./CONTRIBUTING.md)).

## 📄 License

[MIT](./LICENSE)
