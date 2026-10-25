---
trigger: always_on
---

# Clean Principles — Core Rules

These rules are always active. They form the baseline coding standards for every task.

## Naming

- Use **intention-revealing names**: `elapsedDays` not `d`, `isUserActive` not `flag`
- Functions = **verb phrases** (`calculateTotal`, `sendEmail`)
- Classes = **noun phrases** (`UserAccount`, `OrderProcessor`)
- No abbreviations unless universally known (`url`, `id`, `api` are fine; `usrActFlg` is not)

## Functions & Methods

- **One responsibility** per function — if you need "and" to describe it, split it
- **≤ 3 parameters** — more means you need a parameter object
- **No flag arguments** — `process(true)` tells nothing; use two named functions
- Keep functions **small** — if it doesn't fit on screen, it probably does too much

## File Sizing & Modularity

- **≤ 300 lines per file** (soft limit 200, hard limit 400). If a file exceeds 300 lines, it is doing too much — decompose into focused modules, smaller classes, or domain helpers
- **Single primary concept per file** — avoid "junk drawer" files (`utils.ts`, `helpers.py`, `Common.java`) with 30 unrelated functions

## Human-Readable Code (Senior 10+ YoE Mindset)

- **Write code for humans first, compiler second** — optimize for the tired engineer on-call at 2 AM. Cleverness is technical debt; clarity is an asset
- **Guard clauses over nested `if/else`** — return early on invalid conditions. Happy path stays at root indentation (max nesting level ≤ 2)
- **No clever one-liners** — if a chained map/filter/reduce or complex ternary needs a double-take to understand, split it into obvious, named steps
- **Self-documenting conditionals** — extract complex booleans into well-named boolean variables or helper functions (`if (isEligibleForDiscount)` not `if (user.age > 60 && user.membershipYear >= 2 && !user.isSuspended)`)
- **Command-Query Separation (CQS)** — a function either computes/returns a value or changes system state, never both secretly

## Code Quality

- **No magic numbers** — extract to named constants (`MAX_RETRIES = 3` not `3`)
- **No commented-out code** — delete it; git has history
- **Comments explain WHY**, not WHAT — the code is the what
- **No `null`/`None` returns** from functions that return collections — return empty collection
- **Catch specific exceptions** — never bare `except:` / `catch (Exception e)`

## Design

- **DRY**: Don't duplicate business logic — one source of truth per rule
- **YAGNI**: Don't build features nobody asked for yet
- **KISS**: Prefer the simple solution — complexity needs justification
- **SRP**: Each class/module has one reason to change

## Ecosystem Idioms & Conventions

- **Adhere to the target language's idiomatic style** — write native-feeling code, never transplant alien paradigms (e.g., don't write Java-style classes in Go, or C-style loops in Python)
- **Follow framework & community standards** — adhere to official best practices (PSR-12/Laravel conventions for PHP, PEP 8 for Python, Effective Go, React Rules of Hooks, Rust API Guidelines)
- **Respect local toolchain configs** — read and comply with existing project linters and type configs (`tsconfig.json`, `phpstan.neon`, `.eslintrc`, `pyproject.toml`, `Cargo.toml`)
- **Consistency over personal preference** — existing codebase conventions always win. Blend seamlessly with neighboring files

## Before Every Commit

Ask yourself:
1. Can I explain what each function does in one sentence?
2. Would a new team member understand this without asking me?
3. Does this code follow the idiomatic conventions and idioms of this language/framework?
4. Is any file exceeding 300 lines or any function nested more than 2 levels deep?
5. Is there any dead code, commented code, or `TODO` older than this PR?
