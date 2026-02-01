## Code Smells Quick Reference

**Apply when:** Reviewing code, refactoring, or noticing something "feels wrong."

### The Five Categories

| Category | What It Means | Common Smells |
|----------|---------------|---------------|
| **Bloaters** | Code grown too large | Long method, large class, long parameter list |
| **OO Abusers** | Misuse of OO principles | Switch on type, refused bequest |
| **Change Preventers** | Makes changes difficult | Divergent change, shotgun surgery |
| **Dispensables** | Code that can be removed | Dead code, comments explaining bad code, speculative generality |
| **Couplers** | Excessive coupling | Feature envy, inappropriate intimacy, message chains |

### Top 7 Code Smells

| Smell | Symptom | Refactoring |
|-------|---------|-------------|
| **Long Method** | > 10 lines, does multiple things | Extract methods |
| **Large Class** | > 50 lines, many responsibilities | Split into focused classes |
| **Feature Envy** | Method uses another class's data more than its own | Move method to that class |
| **Primitive Obsession** | Using strings/ints for domain concepts (email, ID) | Create value objects |
| **Switch on Type** | `if isinstance()` or type-checking logic | Replace with polymorphism |
| **Inappropriate Intimacy** | Classes know too much about each other | Move methods, extract interface |
| **Speculative Generality** | "Just in case" abstractions | Delete unused code (YAGNI) |

### Quick Detection

```python
# Long Method - Extract smaller functions
def process_order(order):  # Too long
    # validate (lines 1-10)
    # calculate (lines 11-25)
    # save (lines 26-35)
    # notify (lines 36-45)

# Better: Extract each concern
def process_order(order):
    validate_order(order)
    total = calculate_total(order)
    save_order(order, total)
    notify_customer(order)
```

```python
# Primitive Obsession - Use value objects
def send_email(email: str, subject: str):  # Smell
    pass

# Better: Domain type with validation
class Email:
    def __init__(self, value: str):
        if "@" not in value:
            raise ValueError("Invalid email")
        self.value = value

def send_email(email: Email, subject: str):
    pass
```

```python
# Feature Envy - Move to the right class
class Order:
    def calculate_shipping(self, customer):  # Smell
        if customer.country == "US":
            if customer.state == "CA":
                return 10
        return 25

# Better: Move to Customer
class Customer:
    def get_shipping_cost(self) -> int:
        if self.country == "US" and self.state == "CA":
            return 10
        return 25
```

### When You Find a Smell

1. **Confirm it's a problem** - Not all smells need fixing
2. **Ensure test coverage** - Before refactoring
3. **Refactor in small steps** - Keep tests passing
4. **Commit frequently** - Easy to revert if needed

### Prevention

- Follow TDD - Tests reveal design problems early
- Apply SOLID - Prevents structural smells
- Review code - Fresh eyes catch smells
- Refactor continuously - Don't let smells accumulate
