## SOLID Principles Quick Reference

**Apply when:** Writing code, refactoring, reviewing, or designing architecture.

### The Five Principles

| Principle                 | Key Question                                    | Violation Signal                                |
| ------------------------- | ----------------------------------------------- | ----------------------------------------------- |
| **S**ingle Responsibility | Does this have ONE reason to change?            | Need "and" to describe it                       |
| **O**pen/Closed           | Can I extend without modifying?                 | Adding features requires changing existing code |
| **L**iskov Substitution   | Can subtypes replace base types safely?         | Type-checking conditionals                      |
| **I**nterface Segregation | Are clients forced to depend on unused methods? | Empty method implementations                    |
| **D**ependency Inversion  | Do high-level modules depend on abstractions?   | `new ConcreteClass()` in business logic         |

### Red Flags - Stop and Refactor

| Flag                                 | Problem                   | Fix                           |
| ------------------------------------ | ------------------------- | ----------------------------- |
| Class > 50 lines                     | Too many responsibilities | Split into focused classes    |
| Method > 10 lines                    | Doing too much            | Extract helper methods        |
| > 2 instance variables               | Mixed concerns            | Split class                   |
| Using `any` type                     | Type safety               | Use proper types or `unknown` |
| Switch on type/enum                  | Violates OCP              | Use polymorphism or strategy  |
| Primitive for domain (string for ID) | Primitive obsession       | Create value object           |
| Deep object access (a.b.c.d)         | Law of Demeter            | Add delegation methods        |

### Top 5 Code Smells

1. **Long Method** - > 10 lines → Extract Method
2. **Primitive Obsession** - Raw strings for IDs, emails → Value Objects
3. **Feature Envy** - Method uses other class's data more → Move Method
4. **God Component** - React component > 200 lines → Extract Components
5. **Prop Drilling** - Props passed 3+ levels → Context or Composition

### Value Objects Pattern

```typescript
// Instead of: function createUser(email: string, userId: string)
// Use value objects:
class Email {
  private constructor(private readonly value: string) {}
  static create(value: string): Email {
    if (!value.includes('@')) throw new Error('Invalid email');
    return new Email(value.toLowerCase());
  }
}

class UserId {
  private constructor(private readonly value: string) {}
  static create(value: string): UserId {
    if (!value) throw new Error('Invalid user ID');
    return new UserId(value);
  }
}
```

### React-Specific Guidelines

- **Hooks:** Single responsibility - separate fetch, form state, validation
- **Components:** < 200 lines, composition over prop explosion
- **useEffect:** One effect per concern, no derived state

---

For detailed guidance with TypeScript examples, invoke `/solid` skill.
