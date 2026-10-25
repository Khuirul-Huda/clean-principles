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

## Classes

**Small classes with high cohesion — methods should use most instance variables.**

```python
# ❌ BAD: One class, two unrelated concerns — methods only use half the variables
class UserService:
    def __init__(self):
        self.name = ""
        self.email = ""        # Group 1: user identity
        self.token = ""
        self.expires_at = None # Group 2: session auth — unrelated
        self.refresh_token = ""

    def get_display_name(self): return self.name  # uses only Group 1
    def validate_email(self): ...                 # uses only Group 1
    def refresh_session(self): ...                # uses only Group 2
    def is_session_expired(self): ...             # uses only Group 2

# ✅ GOOD: Split into two cohesive classes
class User:
    def __init__(self, name: str, email: str):
        self.name = name
        self.email = email

    def get_display_name(self) -> str: return self.name
    def validate_email(self) -> bool: ...

class UserSession:
    def __init__(self, token: str, expires_at: datetime):
        self.token = token
        self.expires_at = expires_at
        self.refresh_token = ""

    def is_expired(self) -> bool: ...
    def refresh(self) -> None: ...
```

**Class rules:**
- If a method only uses half the instance variables, the class likely has two responsibilities
- Cohesion check: can every method be described using the class name alone?
- Prefer many small, focused classes over one large class

## File Sizing & Modularity

**Files should be focused, coherent, and manageable at a single glance.**

- **Target size:** ≤ 200–300 lines of code (soft limit 200, hard limit 400).
- **Why?** A file > 300 lines is almost always a "God File" or "Kitchen Sink" that mixes multiple concerns (validation, domain logic, persistence, and formatting).
- **The 30-Second Rule:** A developer should be able to scan an entire file and understand its complete mental model in under 30 seconds.

```python
# ❌ BAD: 500-line "God File" order_manager.py
# Contains: Order model, DB queries, Stripe API calls, Email HTML formatting, PDF invoice generation.

# ✅ GOOD: Decomposed into focused, single-purpose files (< 150 lines each)
# order.py             -> Domain entity and state transitions
# order_repository.py  -> Database persistence
# payment_gateway.py   -> Stripe payment integration
# invoice_generator.py -> PDF invoice rendering
# order_notifier.py    -> Email notification dispatch
```

**Decomposition rules:**
- Does this file contain more than one class or export more than 5 primary public functions? Split it.
- Avoid generic junk-drawer files (`utils.py`, `helpers.ts`, `common.js`). Group by domain concept (`date_formatter.py`, `currency.ts`).

## Human-Readable Code (Senior 10+ YoE Standard)

**"Code is written for humans first, compiler second. Optimize for the tired engineer on-call at 2 AM."**

### 1. Guard Clauses Over Nested `if/else` (Kill the Arrow Anti-Pattern)

Deeply nested code forces the reader to maintain a heavy mental stack. Invert checks and exit immediately. Keep indentation levels ≤ 2.

```python
# ❌ BAD: Arrow anti-pattern (nested 4 levels deep)
def register_student(student, course):
    if student is not None:
        if course.has_capacity():
            if not student.is_already_enrolled(course):
                if student.has_passed_prerequisites(course):
                    course.enroll(student)
                    return True
                else:
                    raise PrerequisiteNotMetError()
            else:
                raise AlreadyEnrolledError()
        else:
            raise CourseFullError()
    else:
        raise ValueError("Student cannot be None")

# ✅ GOOD: Guard clauses (Flat, zero mental stack, happy path at root indentation)
def register_student(student: Student, course: Course) -> None:
    if student is None:
        raise ValueError("Student cannot be None")
    if not course.has_capacity():
        raise CourseFullError()
    if student.is_already_enrolled(course):
        raise AlreadyEnrolledError()
    if not student.has_passed_prerequisites(course):
        raise PrerequisiteNotMetError()

    # Happy path runs cleanly at root indentation
    course.enroll(student)
```

### 2. Self-Documenting Conditionals (Encapsulate Predicates)

Don't force readers to mentally evaluate complex boolean logic. Name the business meaning.

```python
# ❌ BAD: Complex boolean soup — reader must decrypt business rules
if user.age >= 60 and user.membership_years >= 2 and not user.has_active_penalties and total > 500:
    apply_senior_loyalty_discount(order)

# ✅ GOOD: Named predicate reveals business intent immediately
is_senior_loyalty_member = user.is_senior and user.is_loyal_member and not user.has_penalties
is_qualifying_order = total > MINIMUM_DISCOUNT_THRESHOLD

if is_senior_loyalty_member and is_qualifying_order:
    apply_senior_loyalty_discount(order)
```

### 3. Clarity Over Cleverness (Ban Dense One-Liners)

Clever code makes the author feel smart; clear code makes the team move fast.

```python
# ❌ BAD: "Clever" one-liner that takes 30 seconds to parse
res = [u["id"] for u in users if u.get("meta", {}).get("tier") == "VIP" and any(o["val"] > 100 for o in u.get("orders", []))]

# ✅ GOOD: Readable, debuggable, easy to step through with a debugger
def is_high_value_vip(user: dict) -> bool:
    if user.get("meta", {}).get("tier") != "VIP":
        return False
    return any(order["val"] > 100 for order in user.get("orders", []))

vip_user_ids = [user["id"] for user in users if is_high_value_vip(user)]
```

### 4. Command-Query Separation (CQS)

A function should either **do something** (mutate state) or **answer something** (return a value pure of side effects), never both in secret.

```python
# ❌ BAD: Secret side-effect in a getter
def get_user_profile(user_id: str) -> UserProfile:
    user = db.find(user_id)
    user.last_login = datetime.now()  # Hidden state mutation!
    db.save(user)
    return user

# ✅ GOOD: Explicit, separated intentions
def get_user_profile(user_id: str) -> UserProfile:
    return db.find(user_id)

def record_user_login(user_id: str) -> None:
    user = db.find(user_id)
    user.last_login = datetime.now()
    db.save(user)
```

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

- **Vertical distance:** Related things stay close. Follow the **Stepdown Rule** — callers above callees, read top-to-bottom like a newspaper: high-level summary first, implementation details below.

  ```python
  # ✅ GOOD: High-level entry point at top, details flow downward
  def process_checkout(cart):
      validate_cart(cart)
      total = calculate_total(cart)
      return charge_payment(total)

  def validate_cart(cart): ...    # detail — below its caller
  def calculate_total(cart): ...  # detail — below its caller
  def charge_payment(total): ...  # detail — below its caller
  ```
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

## Ecosystem Idioms & Framework Conventions

**"Write native-feeling code. Never transplant alien paradigms into a language that doesn't want them."**

A senior engineer does not write Java in Python, C in Go, or procedural PHP in modern Laravel. Every ecosystem has community-tested patterns, conventions, and toolchains that must be respected.

### 1. Language-Specific Idioms

| Language | Anti-Pattern (Alien Paradigm) | Idiomatic Way (Native Paradigm) |
|---|---|---|
| **Python** | Index loops (`for i in range(len(arr)):`), Java getters/setters (`get_name()`) | Direct iteration (`for item in arr:` / `enumerate()`), `@property`, context managers (`with`). Follow PEP 8. |
| **TypeScript / React** | In-place mutations (`user.status = 'ACTIVE'`), `any` types, ignoring hook deps | Immutability (`{ ...user, status: 'ACTIVE' }`), discriminated unions, strict null checks, Rules of Hooks. |
| **Go** | Deep inheritance, panicking for flow control, stuttering names (`user.UserService`) | Small interfaces (1-2 methods), explicit error handling (`if err != nil`), concise naming (`user.Service`). |
| **PHP / Laravel** | Raw DB queries in controllers, bypassing FormRequests, ignoring Eloquent | Eloquent relationships, FormRequest validation, API Resources, Service Container injection. Follow PSR-12. |
| **Rust** | Overusing `.clone()` / `Rc<RefCell<T>>` to bypass borrow checker | Idiomatic borrowing (`&str`, `&[T]`), pattern matching, `Result`/`Option` combinators (`?`, `.map()`). |

### 2. Framework & Tooling Inspection Protocol

Before generating code in any project:
1. **Inspect Toolchain Configs First:** Always check existing linter/formatter/compiler configs before writing a single line:
   - `tsconfig.json` (path aliases like `@/*`, strict mode)
   - `.eslintrc` / `biome.json` / `.prettierrc` (formatting, semi-colons, quotes)
   - `phpstan.neon` / `pint.json` / `phpcs.xml` (PHP static analysis & style)
   - `pyproject.toml` / `ruff.toml` / `setup.cfg` (Python tooling)
2. **Follow the Neighbor Rule (Consistency Wins):**
   - If existing files use kebab-case (`order-card.tsx`), do not introduce PascalCase (`OrderCard.tsx`).
   - Match existing architecture: if the project uses Feature-Sliced Design or Hexagonal Architecture, follow that structure.
3. **Use Framework-Native Primitives:**
   - Don't write custom array utilities if the framework provides them (e.g., Laravel Collections `collect()`, native JS `Array.prototype` methods, Python `itertools`).

## Quick Self-Check

Before committing, ask:
- [ ] Can I understand each function in 5 seconds?
- [ ] Does every name reveal its intent without a comment?
- [ ] Is each function doing exactly one thing?
- [ ] Are all files under 300 lines of code?
- [ ] Are there guard clauses instead of nested `if/else` (max nesting ≤ 2)?
- [ ] Does the code follow idiomatic conventions of the specific language and framework?
- [ ] Are all comments explaining *why*, not *what*?
- [ ] Is there any commented-out code? (delete it)
