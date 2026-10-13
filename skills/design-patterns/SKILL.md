---
name: design-patterns
description: Use when solving recurring structural or behavioral design problems — recognizing when code needs a proven pattern like factory, observer, strategy, or repository.
---

# Design Patterns

Proven solutions to recurring design problems. Use patterns when you recognize the problem — not to show cleverness.

> **Warning:** Patterns add complexity. Only apply when the problem is actually recurring.

## Creational — How Objects Are Created

### Factory Method
**Problem:** Caller shouldn't know which concrete class to instantiate.
```python
# Instead of: notification = EmailNotification(msg)
# Use: notification = NotificationFactory.create("email", msg)

class NotificationFactory:
    _registry = {"email": EmailNotification, "sms": SMSNotification}

    @classmethod
    def create(cls, type: str, message: str) -> Notification:
        if type not in cls._registry:
            raise ValueError(f"Unknown notification type: {type}")
        return cls._registry[type](message)
```

### Builder
**Problem:** Object needs many optional parameters (telescoping constructor).
```python
# Instead of: Query(table, cols, where, limit, offset, order, joins)
query = (QueryBuilder("users")
    .select("id", "name")
    .where("active = true")
    .limit(10)
    .build())
```

### Singleton
**Problem:** Exactly one instance needed (config, connection pool).
```python
class Config:
    _instance = None

    def __new__(cls):
        if not cls._instance:
            cls._instance = super().__new__(cls)
        return cls._instance
```
⚠️ Avoid for anything testable — singletons make tests order-dependent.

---

## Structural — How Objects Are Composed

### Adapter
**Problem:** Make an incompatible interface work with existing code.
```python
# Legacy system returns dict, new system expects object
class LegacyUserAdapter:
    def __init__(self, legacy_dict: dict):
        self._data = legacy_dict

    @property
    def id(self): return self._data["user_id"]
    @property
    def name(self): return self._data["full_name"]
```

### Decorator
**Problem:** Add behavior without modifying the original class.
```python
class LoggingRepository:
    def __init__(self, repo: UserRepository):
        self._repo = repo

    def find(self, id: int) -> User:
        logger.info(f"Finding user {id}")
        result = self._repo.find(id)
        logger.info(f"Found: {result}")
        return result
```

### Repository
**Problem:** Decouple business logic from data access.
```python
class UserRepository(ABC):
    @abstractmethod
    def find_by_id(self, id: int) -> User: ...
    @abstractmethod
    def save(self, user: User) -> None: ...

class SQLUserRepository(UserRepository):
    def find_by_id(self, id: int) -> User:
        row = self.db.query("SELECT * FROM users WHERE id = ?", id)
        return User.from_row(row)
```

---

## Behavioral — How Objects Communicate

### Observer / Event
**Problem:** Notify multiple objects when state changes, without tight coupling.
```python
class EventBus:
    def __init__(self):
        self._handlers: dict[str, list[Callable]] = {}

    def subscribe(self, event: str, handler: Callable): ...
    def publish(self, event: str, data: any): ...

# OrderService doesn't know about EmailService, InventoryService, etc.
event_bus.publish("order.placed", order)
```

### Strategy
**Problem:** Swap algorithms at runtime.
```python
class PaymentProcessor:
    def __init__(self, strategy: PaymentStrategy):
        self._strategy = strategy

    def charge(self, amount: float):
        self._strategy.process(amount)  # Stripe, PayPal, Crypto — all the same interface
```

### Command
**Problem:** Encapsulate requests as objects (for undo/queue/logging).
```python
class Command(ABC):
    @abstractmethod
    def execute(self): ...
    @abstractmethod
    def undo(self): ...

class MoveCommand(Command):
    def execute(self): self.piece.move(self.destination)
    def undo(self): self.piece.move(self.origin)
```

---

## Pattern Selection Guide

| Situation | Pattern |
|-----------|---------|
| Creating objects based on type/condition | Factory Method |
| Complex object construction | Builder |
| Incompatible interface | Adapter |
| Add behavior non-invasively | Decorator |
| Decouple data access | Repository |
| Notify many listeners | Observer |
| Swap algorithms | Strategy |
| Undo/queue operations | Command |

## Anti-Pattern Warning

| Pattern abuse | Sign |
|---------------|------|
| Factory for 1 type | Just use `new` |
| Observer with 1 subscriber | Just call the method |
| Singleton everywhere | You're hiding global state |
| Strategy with 1 strategy | Premature flexibility |
