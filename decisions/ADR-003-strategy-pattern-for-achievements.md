# ADR-003: Use the Strategy Pattern for the Achievement Engine

- **Tags:** taskmaster, architecture, design-patterns, sprint-5

## Context

Sprint 5 of the MVP roadmap is to build the achievement engine. The plan is to ship 5 working achievements at MVP, but the system is designed to grow — I expect to have 15+ achievements by the end of the project, and more after the course ends.

Each achievement is a different rule: "complete your first task", "finish 5 tasks in one day", "complete a task before 6 AM", and so on. They look different but answer the same question: *given the user's stats, is this unlocked?*

I need to pick a structure for this before I start writing code. If I get it wrong, I'll either spend Sprint 5 fighting the design, or I'll have to refactor it later.

## Options I considered

1. **One big `if/else` chain inside `AchievementService`.**
   Easy to start, but adding a new achievement means editing a central method. Breaks the Open/Closed Principle. Becomes unmanageable past ~5 rules.

2. **Strategy pattern — one rule class per achievement.**
   Each rule is its own small class implementing a common interface. Spring auto-discovers them. Adding a new achievement = adding a new class. No central method to edit.

3. **Chain of Responsibility.**
   Rules pass the request along the chain. Overkill — the rules are independent and don't need ordering or short-circuiting.

4. **Rule engine library (Drools, Easy Rules).**
   Industry-grade, but massive overkill for 15 rules. New dependency, new DSL to learn, more deployment complexity. Wrong tool for a course project.

5. **Observer pattern.**
   Right pattern for *notifying* about unlocks (toast, email). Wrong pattern for *deciding* unlocks. I might use Observer later for the notification side.

## Decision

Option 2 — **Strategy pattern**.

The rule logic lives in `com.example.demo.service.achievement`:
- `AchievementRule` interface with `achievementId()` and `isMet(UserStats)`.
- `AchievementChecker` as the context — Spring auto-injects a `List<AchievementRule>`.
- `UserStats` as an immutable input record.
- `UserStatsBuilder` is the only class allowed to read from repositories.
- Each achievement gets its own `@Component` class under `rules/`.

The `AchievementChecker` is a **pure function**: no DB calls, no logging, no I/O. Same input always returns the same output.

## Consequences

**Positive:**
- Adding a new achievement = adding a new class. No edits to existing code.
- Each rule is testable on its own without a database or mocks.
- The pure-function checker can be reused for a future "preview my next achievement" feature without rewriting.
- Forces a clear separation between *deciding* unlocks (pure) and *persisting* them (side-effectful).

**Negative:**
- More files than a single-class approach. For a 5-rule MVP this is a real cost.
- Slight learning curve for anyone unfamiliar with the pattern (mitigated by the module README).

**Mitigation for the "more files" cost:**
- Each rule file is ~10 lines, so the file count is high but the total code volume is the same.
- The benefit kicks in around rule #5 and grows from there. By the time the project ends, the call will look obvious.
