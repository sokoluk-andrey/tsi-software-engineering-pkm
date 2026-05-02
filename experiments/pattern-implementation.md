# Experiment: Implementing TaskMaster's Achievement Engine with the Strategy Pattern via AI

- **Tags:** ai, taskmaster, design-patterns, strategy, gof

## Hypothesis

If I give an AI assistant:
1. A clear instruction to apply the **Strategy** pattern,
2. Hard rules about pure functions, no DB access in rules, immutable inputs,
3. The exact entity types and method signatures from my project,

…the AI will produce a clean, idiomatic Strategy implementation on the first try.

If I leave any of those constraints out, I expect the AI to either fall back to a procedural style under a thin wrapper, or apply the pattern correctly but smuggle DB access into the rule classes.

## The exact prompt I used

> I'm implementing the achievement engine for TaskMaster (Spring Boot 3.4.3, Java 21, JPA, package `com.example.demo`). The MVP needs at least 5 working achievements, and the system is designed to grow to 15+ rules over time.
>
> Implement the rule-checking logic using the **Strategy pattern (GoF behavioral)**. Hard rules:
>
> 1. Create a sub-package `com.example.demo.service.achievement`.
> 2. Define a `UserStats` immutable record holding everything any rule could need (counts, time-of-day, sets of categories, etc.).
> 3. Define an `AchievementRule` interface with `String achievementId()` and `boolean isMet(UserStats stats)`.
> 4. Each achievement gets its own `@Component` rule class under `rules/`. Implement these 15: beginner, time_master, productive_day, super_productive_day, early_bird, night_owl, goal_oriented, weekend_warrior, perfect_week, project_leader, multitasker, organization_master, priority_expert, early_planner, speed_runner.
> 5. `AchievementChecker` is the Strategy context. Spring auto-wires `List<AchievementRule>`. The checker is a **pure function** — no DB, no logging, no I/O, no input mutation.
> 6. A separate `UserStatsBuilder` is the only class allowed to read from repositories. It builds the snapshot once per call.
> 7. The `AchievementService.checkAndGrantAchievements(user, task)` method should: build stats → call checker → loop through returned IDs and persist new achievements.
> 8. JUnit 5 + AssertJ tests for the checker.

I attached the entity classes (`Task`, `User`, `Achievement`) and the `TaskRepository` interface so the AI knew the exact method signatures.

## Method

Two attempts:
- **Attempt 1:** vague prompt — "implement the achievement engine using the Strategy pattern."
- **Attempt 2:** the strict prompt above.

## Results

### Attempt 1 — vague prompt

The AI produced something that *looked* like Strategy on the surface:
- It made an `AchievementRule` interface.
- It made one class per achievement.

But:
- Every rule class had `@Autowired TaskRepository` and `@Autowired AchievementRepository` injected directly.
- Each rule queried the database independently, which would mean a single `checkAndGrantAchievements` call triggers 15+ separate DB round-trips.
- The "rules" weren't pure functions. They were just procedural methods sprinkled across 15 classes.
- No tests were generated unless I asked.

This was Strategy-as-decoration. The shape was right, the substance was wrong.

### Attempt 2 — strict prompt with constraints

The AI produced a much cleaner result:
- `UserStats` as a `record` with all the fields the rules need.
- `UserStatsBuilder` as the only class with repository access. Single DB read pass per check.
- `AchievementChecker` accepting `List<AchievementRule>` injected by Spring. Returns `List<String>`. No state, no I/O.
- 15 rule classes, each ~10 lines, each implementing `isMet(UserStats stats)` with a one-liner.
- JUnit 5 + AssertJ tests covering single unlock, already-unlocked filter, multiple unlocks, no input mutation.

Two things I had to fix manually:
1. The AI initially mismatched some field names with my actual `Task` entity. I had it regenerate using the names from the entity I supplied.
2. It missed the quirk that `Task.month` is stored 0-indexed (matching the JS frontend convention). Once I flagged it in `UserStatsBuilder`, the fix was correct.

## Did the pattern help, or was it overengineering?

Genuinely helped, no overengineering, for two reasons:

1. **Cognitive load per file dropped massively.** Each rule is ~10 lines. To understand "what unlocks Productive Day?" I open one file with one line. With an `if/else` chain I would have been scrolling through a 200-line method.
2. **Adding the next achievement is trivial.** New `@Component` class. No central registration, no `switch` to extend, no risk of breaking a sibling rule. The Open/Closed Principle stops being a textbook concept and becomes the actual experience of working in the code.

It would have been overengineering if there were 2–3 rules. With 15+ and growing, it's the right call. ADR-003 records the decision.

## Takeaways

1. **AI applies design patterns shallowly by default.** Without strict constraints, "use Strategy" becomes "split the methods across classes" — same procedural mess, more files. The pattern is in the *constraints*, not the *names*.
2. **Pure-function constraints are the lever.** The single most useful instruction was "no DB calls inside rules." That one rule forces the entire structure to organize itself correctly.
3. **The `UserStats` snapshot pattern is reusable.** Pre-compute → pass into pure functions. I'll use this for the timer module too.
4. **Over-specification is cheaper than re-prompting.** Attempt 1 took 25 minutes including the back-and-forth to fix what was wrong. Attempt 2 took 8 minutes including review. The "wasted" time writing the strict prompt paid for itself 3x.

## Follow-up

- Add a section to `AGENTS.md` in the product repo: "When asking AI to apply a design pattern, also list the *constraints* that make the pattern meaningful (no DB in rules, no mutation, etc.). The pattern name alone is not enough."
- Apply the same UserStats-snapshot approach to the timer module in Sprint 4.
