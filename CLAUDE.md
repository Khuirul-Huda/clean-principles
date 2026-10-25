# clean-principles — Coding Standards

These rules are loaded from the `clean-principles` plugin.

## Active Skills

The following skills are available on-demand. Ask me to use them when relevant:

- **solid-principles** — SOLID design principles (SRP, OCP, LSP, ISP, DIP)
- **clean-code** — Naming, functions, comments, formatting (Clean Code by Robert Martin)
- **dry-kiss-yagni** — DRY, KISS, YAGNI — keep code lean and simple
- **design-patterns** — Factory, Observer, Strategy, Repository, and more
- **architecture-principles** — SoC, layered arch, hexagonal, cohesion/coupling
- **testing-principles** — TDD, FIRST, AAA, test doubles, testing pyramid
- **clarify-before-build** — Disambiguate scope before building ("simple", "quick", "production-ready")

## Always-On Rules

### Naming
- Use intention-revealing names (`elapsedDays` not `d`, `isUserActive` not `flag`)
- Functions = verb phrases (`calculateTotal`, `sendEmail`)
- Classes = noun phrases (`UserAccount`, `OrderProcessor`)
- No cryptic abbreviations

### Functions
- One responsibility per function — if you need "and" to describe it, split it
- ≤ 3 parameters — more needs a parameter object
- No flag arguments — use two named functions instead
- No side effects beyond the stated purpose

### File Sizing & Modularity
- ≤ 300 lines per file (soft limit 200, hard limit 400) — split larger files into cohesive modules
- Avoid generic junk-drawer files (`utils.ts`, `helpers.py`)

### Human-Readable Code (Senior 10+ YoE Standard)
- Write for humans first, compiler second — optimize for the engineer on-call at 2 AM
- Guard clauses over nested `if/else` (max indentation level ≤ 2)
- No clever one-liners or complex chained expressions
- Self-documenting conditionals — encapsulate predicates into named booleans/functions
- Command-Query Separation (CQS) — never mutate state inside getters/queries

### Code Quality
- No magic numbers — extract to named constants
- No commented-out code — delete it, git has history
- Comments explain WHY, not WHAT
- No `null`/`None` returns from functions that return collections — return empty collection
- Catch specific exceptions — never bare `except:` or `catch (Exception e)`

### Design
- **DRY**: One source of truth per business rule
- **YAGNI**: Don't build features nobody asked for yet
- **KISS**: Prefer simple over clever
- **SRP**: Each class/module has one reason to change

## Before Every Commit

Ask yourself:
1. Can I explain what each function does in one sentence?
2. Would a new team member understand this without asking me?
3. Is any file exceeding 300 lines or any function nested more than 2 levels deep?
4. Is there any dead code, commented code, or `TODO` older than this PR?
