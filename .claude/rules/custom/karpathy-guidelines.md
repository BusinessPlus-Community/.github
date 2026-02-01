## Karpathy Guidelines Quick Reference

**Purpose:** Apply LLM coding best practices when writing, reviewing, or refactoring code.

### When to Apply

| Trigger                         | Action                      |
| ------------------------------- | --------------------------- |
| Starting complex implementation | Review all 4 principles     |
| Editing existing code           | Apply Surgical Changes      |
| Task feels unclear              | Apply Think Before Coding   |
| Designing a solution            | Apply Simplicity First      |
| About to mark complete          | Apply Goal-Driven Execution |

### The Four Principles

1. **Think Before Coding** - Don't assume. Surface confusion. Ask about ambiguities.
2. **Simplicity First** - Build only what's requested. No speculative features.
3. **Surgical Changes** - Touch only what you must. Clean up only your own mess.
4. **Goal-Driven Execution** - Define success criteria. Verify, don't assume.

### Quick Checks

| Principle   | Verification Question                           |
| ----------- | ----------------------------------------------- |
| Think First | "Have I stated my assumptions explicitly?"      |
| Simplicity  | "Is every feature explicitly requested?"        |
| Surgical    | "Does every changed line trace to the request?" |
| Goal-Driven | "How will I prove this works?"                  |

---

For detailed guidance with examples and decision trees, invoke `/karpathy-guidelines` skill.
