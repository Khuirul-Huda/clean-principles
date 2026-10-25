<div align="center">

# 🧼 clean-principles

**Senior-level software engineering standards & architectural principles for AI coding agents.**

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg?style=flat-square)](./LICENSE)
[![agentskills.io](https://img.shields.io/badge/agentskills.io-compatible-2ea44f?style=flat-square&logo=github)](https://agentskills.io)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg?style=flat-square)](./CONTRIBUTING.md)
[![Agents Supported](https://img.shields.io/badge/Agents-Claude_Code_•_Antigravity_•_Cursor_•_Copilot_•_Codex-8a2be2?style=flat-square)](https://github.com/Khuirul-Huda/clean-principles)
[![GitHub Stars](https://img.shields.io/github/stars/Khuirul-Huda/clean-principles?style=flat-square&color=yellow)](https://github.com/Khuirul-Huda/clean-principles/stargazers)

<p align="center">
  <a href="#-the-philosophy">Philosophy</a> •
  <a href="#-quick-install">Quick Install</a> •
  <a href="#-comparison-with-vs-without">Comparison</a> •
  <a href="#-what-is-included">What's Included</a> •
  <a href="#-repository-structure">Structure</a> •
  <a href="#-contributing">Contributing</a>
</p>

</div>

---

## 💡 The Philosophy: Senior 10+ YoE Mindset

By default, AI coding agents write code that compiles, but is painful for humans to maintain: 800-line monolithic files, 6 levels of nested `if/else` staircases, cryptic abbreviations, and foreign paradigms (like writing Java in Python).

**`clean-principles`** injects an uncompromising **10+ Year Senior Engineer standard** directly into the agent's subconscious reasoning layer:

> *"Write code for humans first, compiler second. Optimize for the tired on-call engineer debugging an outage at 2 AM."*

### Core Engineering Tenets
1. **The 300-Line Rule:** Files must stay focused ($\le 300$ lines). A file exceeding 300 lines is a code smell signaling that it's time to decompose into cohesive modules.
2. **Flattened Control Flow (Anti-Arrow):** Kill the Arrow Anti-Pattern. Always use **Guard Clauses** and exit early. The happy path stays at root indentation (max nesting level $\le 2$).
3. **Self-Documenting Code:** Ban complex boolean soups and dense one-liners. Encapsulate business logic into intention-revealing predicates and named constants.
4. **Ecosystem & Idiom First:** Never transplant alien paradigms. Adhere strictly to language idioms (**PEP 8**, **PSR-12/Laravel**, **Effective Go**, **React Rules of Hooks**, **Rust API Guidelines**) and respect local linter/formatter configurations.

---

## ⚡ Quick Install

### 1. Via Marketplace (Recommended)

| Agent Platform | Installation Command |
|---|---|
| **Antigravity (AGY)** | `agy plugin install https://github.com/Khuirul-Huda/clean-principles` |
| **Claude Code** | `/plugin marketplace add Khuirul-Huda/clean-principles`<br>`/plugin install clean-principles@clean-principles` |
| **Cursor / Windsurf** | Add `AGENTS.md` to your project root or workspace instructions. |
| **OpenAI Codex** | `codex plugin marketplace add Khuirul-Huda/clean-principles`<br>`codex plugin add clean-principles@clean-principles` |
| **GitHub Copilot CLI** | `copilot plugin marketplace add Khuirul-Huda/clean-principles`<br>`copilot plugin install clean-principles@clean-principles` |
| **npm / agentskills** | `npx agentskills export clean-principles` |

### 2. Universal / Project Root Install

For any AI tool or IDE reading `AGENTS.md` (Cursor, Aider, Windsurf, Cline), copy [`AGENTS.md`](./AGENTS.md) directly into your repository root:

```bash
curl -sSL https://raw.githubusercontent.com/Khuirul-Huda/clean-principles/main/AGENTS.md -o AGENTS.md
```

---

## ⚖️ Comparison: With vs. Without `clean-principles`

| Dimension | ❌ Without Plugin (Default AI Behavior) | ✅ With `clean-principles` |
|---|---|---|
| **Class & File Sizing** | 800-line monolithic "God Files" mixing database, UI, validation, and API calls. | Strict **SRP** & **DIP**. Target **≤ 300 lines/file**; cleanly decomposed into cohesive single-purpose files. |
| **Control Flow & Nesting** | Deeply nested `if/else` staircases (Arrow Anti-Pattern, nesting 4–6 levels deep). | **Guard Clauses**: Exit early on invalid states. Flat structure, happy path at root indentation (nesting ≤ 2). |
| **Language & Idioms** | Writes "Java in Python" or procedural code in Laravel; ignores project linters. | **Idiom-first & Toolchain-aware**: Respects PEP 8, PSR-12, Effective Go, and reads `tsconfig`/`phpstan` configs first. |
| **API & Method Design** | Boolean flag arguments (`process(true)`), cryptic names (`u_id`, `usrActFlg`). | Expressive verb phrases (`processStandard()`, `processPromotional()`), intention-revealing names. |
| **Ambiguous Prompts** | Speculative code expansion; outputs 1,500+ tokens of hybrid guesswork. | **Clarify before build**: Asks 1 concise spectrum question (~120 tokens) before touching code. |
| **Error Handling** | Swallows errors (`except: return None`) or raises generic `Exception`. | Specific custom domain exceptions, fails fast, returns empty collections instead of `null`/`None`. |
| **Over-Engineering** | Generates premature factories and abstract bridges for simple 20-line scripts. | Pragmatic guardrails: explicit *"When NOT to apply"* rules (KISS > patterns for small scripts). |
| **Testability** | Tight coupling to external services; hard to test without heavy mocks. | Inverted dependencies allow blazing fast, in-memory unit testing. |

---

## 🔍 Code Transformation Example

````carousel
```python
# ❌ BEFORE: Default AI Output (Arrow Anti-Pattern, Magic Numbers, Mixed Concerns)
def enroll_student(student, course, db, mailer):
    if student is not None:
        if course.has_capacity():
            if not student.has_disciplinary_record:
                if applicant.gpa >= 3.5 and applicant.income < 5000000:
                    db.execute("INSERT INTO enrollments VALUES (?, ?)", student.id, course.id)
                    student.status = "ENROLLED"
                    db.save(student)
                    mailer.send(student.email, "Welcome", "Enrolled successfully")
                    return True
                else:
                    return False
            else:
                return False
        else:
            raise Exception("Course full")
    else:
        return False
```
<!-- slide -->
```python
# ✅ AFTER: With clean-principles (Guard Clauses, CQS, Self-Documenting Predicates)
MIN_GPA_THRESHOLD = 3.5
MAX_FAMILY_INCOME = 5_000_000

def is_eligible(student: Student) -> bool:
    if student is None or student.has_disciplinary_record:
        return False
    return student.gpa >= MIN_GPA_THRESHOLD and student.income < MAX_FAMILY_INCOME

class EnrollmentService:
    def __init__(self, db: Database, mailer: Mailer):
        self._db = db
        self._mailer = mailer

    def enroll(self, student: Student, course: Course) -> bool:
        if not course.has_capacity():
            raise CourseCapacityExceededError(f"Course {course.id} is full.")
        if not is_eligible(student):
            return False

        # Happy path at root indentation
        self._persist_enrollment(student, course)
        self._send_confirmation(student)
        return True
```
````

---

## 📦 What's Included

### 🛡️ Always-On Rules (Loaded Every Turn)
* **[`rules/core-principles.md`](./rules/core-principles.md):** Baseline standards for naming, function sizing, file line limits ($\le 300$ lines), guard clauses, error handling, and pre-commit checklists.
* **[`AGENTS.md`](./AGENTS.md) / [`CLAUDE.md`](./CLAUDE.md):** Standard cross-agent configurations recognized across all major agent frameworks.

### 🧠 7 On-Demand Principle Skills (Loaded Dynamically)

| Skill | Trigger / When to Apply | Focus Areas |
|---|---|---|
| [`clean-code`](./skills/clean-code/SKILL.md) | Naming, writing functions, refactoring messy code | 300-Line Rule, Guard Clauses, Senior 10+ YoE Standard, Ecosystem Idioms |
| [`solid-principles`](./skills/solid-principles/SKILL.md) | Designing classes/modules, interfaces, coupling | SRP, OCP, LSP, ISP, Dependency Inversion (DIP) |
| [`dry-kiss-yagni`](./skills/dry-kiss-yagni/SKILL.md) | Seeing duplication, premature abstraction | Single Source of Truth, Simplest Solution, No "Just-in-Case" code |
| [`design-patterns`](./skills/design-patterns/SKILL.md) | Recurring structural & behavioral challenges | Factory, Strategy, Observer, Repository, Facade, Template Method |
| [`architecture-principles`](./skills/architecture-principles/SKILL.md) | System boundaries, splitting packages | Layered, Hexagonal/Clean Architecture, High Cohesion, Loose Coupling |
| [`testing-principles`](./skills/testing-principles/SKILL.md) | Writing tests, test design, brittle suites | TDD, FIRST, AAA Pattern, Test Doubles, Testing Pyramid |
| [`clarify-before-build`](./skills/clarify-before-build/SKILL.md) | Ambiguous prompt keywords ("simple", "clean") | Multi-choice disambiguation before generating 1,000+ tokens of guesswork |

---

## 📂 Repository Structure

```text
clean-principles/
├── plugin.json                     # Antigravity (AGY) plugin manifest
├── package.json                    # npm / agentskills manifest
├── AGENTS.md                       # Always-on rules for universal AI agents
├── CLAUDE.md                       # Claude Code entry point
├── .claude-plugin/                 # Claude Code marketplace & plugin manifests
├── .agents/                        # Antigravity & Codex marketplace manifests
├── .github/                        # Copilot CLI marketplace manifest
├── .cursor-plugin/                 # Cursor plugin manifest
├── .codex-plugin/                  # OpenAI Codex plugin manifest
├── rules/
│   └── core-principles.md          # Baseline always-on rules (300 lines, guard clauses)
└── skills/                         # 7 comprehensive principle skills
    ├── clean-code/
    ├── solid-principles/
    ├── dry-kiss-yagni/
    ├── design-patterns/
    ├── architecture-principles/
    ├── testing-principles/
    └── clarify-before-build/
```

---

## 🤝 Contributing

Contributions, issues, and feature requests are welcome!
Skills follow the [agentskills.io specification](https://agentskills.io/specification). Commit messages follow [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/) (see [`CONTRIBUTING.md`](./CONTRIBUTING.md)).

## 📄 License

This project is licensed under the [MIT License](./LICENSE).
