---
name: clean-code
description: Use when naming variables/functions/classes, writing functions, or reviewing code that feels hard to read or understand at a glance.
---

# Clean Code

Principles for writing code that humans can read, understand, and maintain.

## Naming

**Names should reveal intent — no abbreviations, no mystery.**

| Rule | Bad | Good |
|------|-----|------|
| Pronounceable names | `genymdhms` | `generatedTimestamp` |
| Searchable names | `e`, `d` | `event`, `elapsedDays` |
| Reveal intent | `theList[4]` | `flaggedCells` |
| No type in name | `nameString` | `name` |
| Distinguish meaningfully | `data1`, `data2` | `userData`, `orderData` |
| Functions: verb phrases | `processIt()` | `saveUser()`, `isValid()` |
| Classes: noun phrases | `Manager` (vague) | `UserAccount`, `OrderParser` |

## Functions

**Small. Do one thing. One level of abstraction.**

```python
# ❌ BAD: Does three things, mixed abstraction levels
def process_order(order):
    # Validate
    if not order.items: raise ValueError("Empty order")
    # Calculate
    total = sum(i.price * i.qty for i in order.items)
    tax = total * 0.1
    # Persist
    db.execute("INSERT INTO orders ...", order.id, total + tax)

# ✅ GOOD: Each function does one thing
def process_order(order):
    validate_order(order)
    total = calculate_total(order)
    save_order(order, total)

def calculate_total(order) -> float:
    subtotal = sum(i.price * i.qty for i in order.items)
    return subtotal + calculate_tax(subtotal)
```

**Function rules:**
- ≤ 3 parameters (more = needs a parameter object)
- No side effects beyond stated purpose
- No flag arguments (`process(true)` → two functions)
- Command/Query separation: either *do* something or *return* something, not both

## Comments

**Comments explain WHY, not WHAT. Good code is self-documenting.**

```python
# ❌ BAD: Comment repeats the code
# Increment i by 1
i += 1

# ❌ BAD: Commented-out code (use git instead)
# old_process(data)
new_process(data)

# ✅ GOOD: Explains business reason (non-obvious)
# Retry exactly 3 times per PCI-DSS §3.2 compliance requirement
MAX_PAYMENT_RETRIES = 3

# ✅ GOOD: Warns about non-obvious consequence
# Don't call close() twice — underlying socket will raise OSError
```

## Formatting

**Code is read more than written — optimize for the reader.**

- **Vertical distance:** Related things stay close. Caller above callee.
- **Horizontal:** Keep lines short (≤ 120 chars). Don't align assignments — it's visual noise.
- **One concept per line.** No `a = b = c = 0`.
- **Blank lines** = paragraph breaks = logical separation.

## Error Handling

```python
# ❌ BAD: Swallowed exception, null returns
def find_user(id):
    try:
        return db.get(id)
    except:
        return None  # Caller has no idea what went wrong

# ✅ GOOD: Specific exception, meaningful message
def find_user(id: int) -> User:
    try:
        return db.get(id)
    except DatabaseConnectionError as e:
        raise ServiceUnavailableError(f"Cannot fetch user {id}") from e
    except UserNotFoundError:
        raise  # Let caller decide
```

**Rules:**
- Don't return `null` — return empty collections or raise exceptions
- Don't pass `null` as arguments
- Catch specific exceptions, not bare `except`/`catch`

## Quick Self-Check

Before committing, ask:
- [ ] Can I understand each function in 5 seconds?
- [ ] Does every name reveal its intent without a comment?
- [ ] Is each function doing exactly one thing?
- [ ] Are all comments explaining *why*, not *what*?
- [ ] Is there any commented-out code? (delete it)
