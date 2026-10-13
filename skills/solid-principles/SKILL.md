---
name: solid-principles
description: Use when designing classes, modules, or functions — especially when code feels hard to extend, test, or change without breaking other parts.
---

# SOLID Principles

Five principles for writing maintainable, extensible object-oriented code.

## Quick Reference

| Principle | Rule | Smell it fixes |
|-----------|------|----------------|
| **S** — Single Responsibility | One class = one reason to change | God classes, methods doing too much |
| **O** — Open/Closed | Open for extension, closed for modification | Switch/if-chains that grow with features |
| **L** — Liskov Substitution | Subtypes must be substitutable for base types | Subclass that breaks parent's contract |
| **I** — Interface Segregation | No client should depend on methods it doesn't use | Fat interfaces with unimplemented methods |
| **D** — Dependency Inversion | Depend on abstractions, not concretions | Hard-coded dependencies, untestable code |

## S — Single Responsibility

**One reason to change** = one actor (user, admin, system) that can demand changes.

```python
# ❌ BAD: User class handles data + persistence + email
class User:
    def get_name(self): ...
    def save_to_db(self): ...       # DB concern
    def send_welcome_email(self): ...  # Email concern

# ✅ GOOD: Separate concerns
class User:
    def get_name(self): ...

class UserRepository:
    def save(self, user: User): ...

class UserMailer:
    def send_welcome(self, user: User): ...
```

## O — Open/Closed

**Extend behavior by adding new code, not modifying existing code.**

```python
# ❌ BAD: Add new shape? Edit this function.
def area(shape):
    if shape.type == "circle": return π * shape.r**2
    if shape.type == "rect": return shape.w * shape.h

# ✅ GOOD: Add new shape? Just add a new class.
class Circle:
    def area(self): return π * self.r**2

class Rectangle:
    def area(self): return self.w * self.h
```

## L — Liskov Substitution

**If S extends T, you can use S wherever T is expected — same behavior, no surprises.**

```python
# ❌ BAD: Square breaks Rectangle's contract
class Rectangle:
    def set_width(self, w): self.width = w
    def set_height(self, h): self.height = h

class Square(Rectangle):
    def set_width(self, w): self.width = self.height = w  # Surprise!

# ✅ GOOD: Don't inherit — use composition or a shared abstraction
class Shape(ABC):
    @abstractmethod
    def area(self) -> float: ...
```

## I — Interface Segregation

**Split fat interfaces into focused ones. Clients only depend on what they use.**

```typescript
// ❌ BAD: Printer forced to implement fax
interface Machine {
  print(): void;
  scan(): void;
  fax(): void;  // Not all machines have this
}

// ✅ GOOD: Focused interfaces
interface Printer { print(): void; }
interface Scanner { scan(): void; }
interface FaxMachine { fax(): void; }
```

## D — Dependency Inversion

**High-level modules define the interface. Low-level modules implement it.**

```python
# ❌ BAD: OrderService directly depends on MySQLDatabase
class OrderService:
    def __init__(self):
        self.db = MySQLDatabase()  # Hard dependency

# ✅ GOOD: Depend on abstraction, inject the implementation
class OrderService:
    def __init__(self, db: DatabaseInterface):
        self.db = db  # Testable, swappable
```

## Common Mistakes

| Mistake | Fix |
|---------|-----|
| Splitting by file, not by responsibility | Ask "who would request this change?" |
| Making everything extensible upfront | Apply OCP only when you see the need (YAGNI) |
| Deep inheritance hierarchies | Prefer composition over inheritance |
| One mega-interface | Split by client usage |
| `new ConcreteClass()` inside business logic | Use constructor injection |
