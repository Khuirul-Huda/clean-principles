# clean-principles — Coding Standards

These rules are loaded from the `clean-principles` plugin.

## Available Skills

The following skills are available on-demand:

- **solid-principles** — SOLID design principles (SRP, OCP, LSP, ISP, DIP)
- **clean-code** — Naming, functions, comments, formatting
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
- One responsibility per function
- ≤ 3 parameters
- No flag arguments — use two named functions
- No hidden side effects

### Code Quality
- No magic numbers — use named constants
- No commented-out code — delete it
- Comments explain WHY, not WHAT
- No `null`/`None` returns from functions that return collections — return empty collection
- Catch specific exceptions only

### Design
- **DRY**: One source of truth per business rule
- **YAGNI**: Don't build features nobody asked for yet
- **KISS**: Simple over clever
- **SRP**: One reason to change per module

## Before Every Commit

Ask yourself:
1. Can I explain what each function does in one sentence?
2. Would a new team member understand this without asking me?
3. Is there any dead code, commented code, or `TODO` older than this PR?
