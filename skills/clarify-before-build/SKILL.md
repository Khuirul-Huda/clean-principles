---
name: clarify-before-build
description: Use when a task contains ambiguous scope words like "production-ready", "simple", "basic", "quick", "proper", or "clean" — before writing any code.
---

# Clarify Before Build

**When scope is ambiguous, ask one targeted question before writing a single line.**

## The Core Problem

Users often say "simple" but mean different things:
- "Simple" = 5 lines, just make it work
- "Simple" = clean architecture, but not over-engineered
- "Simple" = production-ready — they assumed you'd know

Agents often interpret "production-ready" as license to add everything they know belongs in production code. Both assumptions cause rework.

## Ambiguous Scope Triggers

Stop and ask when the request contains:

| Trigger word | What's ambiguous |
|---|---|
| "production-ready" | HTML vs plain text? Retry logic? Factory? Logging level? |
| "simple" / "basic" | 10 lines or 100? Error handling included? |
| "quick" | Throwaway prototype or will this live in the codebase? |
| "proper" / "clean" | Their definition, not yours |
| "make it work" | Just the happy path, or error cases too? |
| No qualifier at all | Assume simple until told otherwise |

## The Ask

One question. Not five. Pick the one that unlocks the most decisions:

```
✅ GOOD — one targeted question:
"Before I start: should this be a quick working implementation
or production-grade (error handling, logging, config via env vars)?
The structure will be quite different."

❌ BAD — interrogation:
"What error handling do you want? What about logging?
Should I use SMTP or an email provider? Do you need retries?
What about HTML vs plain text? Should I add a factory?"
```

## The Spectrum — Offer Two Endpoints

Instead of open-ended, offer two concrete options and let them pick:

```
"Quick (20 lines, works for the happy path):
  → csv.reader, returns list of dicts, no error handling

Production-grade (80+ lines):
  → type coercion, encoding detection, row validation,
    error reporting, streaming for large files

Which direction?"
```

This is faster than free-form answers and surfaces assumptions immediately.

## When NOT to Ask

Don't ask when:
- Task is clearly scoped ("write a one-liner to reverse a string")
- You've already aligned on the approach in this session
- Clarification is more disruptive than just building the simpler version and asking after

**Default when unsure:** Build the simplest version that works, then explicitly say what you left out and why:

```
"I kept this simple — no error handling or streaming.
Let me know if you need those."
```

This is faster than asking and delivers something concrete to react to.

## Common Agent Mistakes

| Mistake | What agents say | What to do instead |
|---------|----------------|-------------------|
| Silent scope expansion | *"This is production-correct"* | Ask first or state what you added and why |
| Assuming "simple" = no quality | Build throwaway code when user wanted clean | Ask or default to clean-but-minimal |
| Over-asking | 5 questions before writing anything | Ask ONE question max |
| Never asking | Implements full stack for a one-liner request | Catch ambiguous triggers above |

## After Building — Always State What You Left Out

Even when you don't ask upfront, always close with:

```
"Left out: retry logic, HTML email, env-var factory.
Add any of these? Just say which."
```

This gives the user control without requiring them to know what to ask for.
