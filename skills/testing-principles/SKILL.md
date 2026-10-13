---
name: testing-principles
description: Use when writing tests, deciding what to test, or when tests feel brittle, slow, or hard to maintain.
---

# Testing Principles

Principles for writing tests that give confidence without becoming a maintenance burden.

## The Testing Pyramid

```
        ▲  E2E / Integration   (few, slow, high confidence)
       ▲▲▲  Integration Tests  (some, moderate speed)
      ▲▲▲▲▲  Unit Tests        (many, fast, isolated)
```

**Invert the pyramid = pain:** Lots of E2E tests → slow CI, flaky builds, hard to debug.

---

## F.I.R.S.T. Properties of Good Tests

| Property | Rule |
|----------|------|
| **Fast** | Unit tests in ms. Full suite in < 10 min. |
| **Independent** | No test depends on another's state. Any order = same result. |
| **Repeatable** | Same result every time. No flakiness. |
| **Self-validating** | Pass or fail — no manual inspection. |
| **Timely** | Written alongside (or before) production code. |

---

## What to Test

```
✅ Test:                          ❌ Don't test:
  Business logic                   Private methods (test via public API)
  Edge cases & boundaries          Framework internals
  Error paths                      Getters/setters with no logic
  Integration between layers       Implementation details (how, not what)
  Public API contracts             Third-party library behavior
```

**Test behavior, not implementation:**
```python
# ❌ BAD: Tests implementation (breaks if you rename _calculate_discount)
assert order._calculate_discount(100) == 10

# ✅ GOOD: Tests behavior (survives refactoring)
assert order.total_with_discount() == 90
```

---

## Arrange-Act-Assert (AAA)

Every test has three clear sections:

```python
def test_order_applies_discount_for_premium_users():
    # Arrange — set up state
    user = User(tier="premium")
    order = Order(user=user, items=[Item(price=100)])

    # Act — do the thing
    total = order.calculate_total()

    # Assert — verify outcome
    assert total == 90  # 10% premium discount applied
```

**One assertion concept per test** — multiple asserts OK if they all verify the same behavior.

---

## Test Doubles

| Double | Use when |
|--------|----------|
| **Stub** | Replace dependency that returns values (no verification) |
| **Mock** | Verify a call was made (with specific args) |
| **Fake** | Lightweight working implementation (in-memory DB) |
| **Spy** | Real object that records calls |

```python
# Stub: control the return value
user_repo = Mock()
user_repo.find.return_value = User(id=1, name="Alice")

# Mock: verify interaction happened
email_service = Mock()
order_service.place_order(order)
email_service.send_confirmation.assert_called_once_with(order)
```

**Prefer fakes over mocks** for complex collaborators — they're less brittle.

---

## TDD — Red → Green → Refactor

1. **Red:** Write a failing test for the behavior you want
2. **Green:** Write the minimum code to make it pass
3. **Refactor:** Clean up, keeping tests green

```
❌ Write code → Write tests   (tests document what you wrote, not what you want)
✅ Write test → Write code    (tests drive the design)
```

**TDD benefit:** Forces small, focused functions. Hard-to-test code = bad design signal.

---

## Anti-Patterns

| Anti-pattern | Problem | Fix |
|--------------|---------|-----|
| **Test the moon** | Testing too much in one test | Split into focused tests |
| **Brittle tests** | Break on every refactor | Test behavior, not internals |
| **Mystery guest** | Test state set in fixture file | Arrange state in the test |
| **Slow tests** | Full DB/network in unit tests | Use fakes/stubs |
| **Flaky tests** | Timing/order-dependent | Make truly independent |
| **Test that always passes** | No real assertion | Assert the actual value |

---

## Quick Self-Check

- [ ] Does each test have one clear reason to fail?
- [ ] Can tests run in any order?
- [ ] Do tests verify behavior, not implementation?
- [ ] Can you understand what each test proves from its name alone?
- [ ] Are slow tests isolated from fast tests?
