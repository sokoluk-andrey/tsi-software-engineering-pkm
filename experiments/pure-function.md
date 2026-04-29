# Experiment: Generating a Pure Function with AI from BDD + Mermaid

- **Tags:** ai, taskmaster, pure-functions, bdd

## Hypothesis

If I give an AI assistant a clear BDD specification and a Mermaid diagram for the achievement-check logic and I explicitly ask for a pure function, it will produce correct, side-effect-free code on the first try. If I leave any of these inputs vague, it will hallucinate behavior or sneak in side effects.

## Method

I picked the "achievement unlock check" feature from my TaskMaster MVP. I prepared:

1. The BDD requirements (`docs/requirements/feature_achievement_unlock.md`).
2. The Mermaid sequence and flow diagrams (`docs/architecture/flow_achievement_unlock.md`).
3. A strict prompt asking for a pure function in Java.

Then I ran two attempts:
- **Attempt 1:** vague prompt, no BDD, no diagram. Just "write me a function that checks user achievements."
- **Attempt 2:** strict prompt with full BDD + Mermaid + the pure function constraint.

## The prompt I used (Attempt 2)

> I am building TaskMaster, a Spring Boot + Vue task manager. I need to implement the achievement-check logic.
>
> Here are the requirements (BDD format):
>
> [pasted full content of feature_achievement_unlock.md]
>
> Here is the architecture flow (Mermaid):
>
> [pasted full content of flow_achievement_unlock.md]
>
> Write the logic for this feature as a **Pure Function** in Java 21. Strict rules:
> - No side effects. No database calls. No logging. No I/O.
> - Stateless. Same input must always produce the same output.
> - Predictable output: returns a `List<String>` of newly unlocked achievement IDs.
> - Method signature: `public static List<String> checkUnlocked(UserStats stats, List<AchievementRule> rules, Set<String> alreadyUnlocked)`.
> - Use `java.util.function.Predicate<UserStats>` for the rule condition.
> - Do not modify any of the input arguments.
> - Add unit tests covering all three acceptance criteria from the BDD.

## Results

### Attempt 1 — vague prompt

The AI generated a function called `checkAndUnlockAchievements` that:
- Took only a `userId` as parameter.
- Called a fake `achievementRepository.findAll()` inside the function.
- Called `userAchievementRepository.save(...)` to persist new unlocks.
- Sent a notification via a fake `notificationService.send(...)`.
- Returned `void`.

This is the textbook anti-pattern the lecture warned about. It looked like it "worked", but it was full of hidden side effects and was untestable without mocking three services.

### Attempt 2 — strict prompt with BDD + Mermaid

The AI produced a pure function on the first try. It:
- Took exactly the parameters I asked for.
- Used `Predicate<UserStats>` to evaluate each rule.
- Returned an immutable `List<String>` (`List.copyOf(...)`).
- Did not call any repository, service, or logger.
- Generated three JUnit 5 tests, one per acceptance criterion, with no mocks needed.

The only thing I had to fix manually was a small detail: the AI initially returned the raw `ArrayList` and I asked for `List.copyOf(...)` to make the result immutable. Otherwise it was correct.

## Did I have to adjust the BDD or diagram?

I had to make **one** clarification to my BDD before Attempt 2 worked cleanly. My original AC2 was ambiguous — it said "the system does not unlock the achievement again" but didn't say *what should be returned*. The AI in an earlier draft returned the achievement again with a `wasAlreadyUnlocked: true` flag. I rewrote AC2 to state explicitly: "the system does not return 'Half Century' again". After that, the AI behaved correctly.

The Mermaid diagrams did not need any changes. The flowchart was especially valuable — the AI clearly followed its branching logic in the generated code.

## Takeaways

1. **Vague prompts produce code with side effects by default.** AI assumes "real-world" patterns (services, repos, notifications) unless told otherwise.
2. **The word "pure function" alone is not enough.** I had to also list the bans: no DB, no logging, no I/O, no input mutation. AI obeys explicit rules better than vibes.
3. **BDD pays off twice.** Once when writing it (it forces me to think clearly), and again when feeding it to AI (it removes ambiguity).
4. **Mermaid is a force multiplier.** The AI clearly used my flowchart as a structural guide. The generated code matched the diagram's branching almost exactly.
5. **One ambiguous word in the BDD is enough to make the AI guess wrong.** My AC2 fix proved it.

## Follow-up

- Use this prompt template for the next pure-function feature (probably timer duration calculation).
- Add a section to `AGENTS.md` in the product repo: "When asking AI for a pure function, always ban DB / logging / I/O / mutation explicitly."
- Move the `AchievementChecker` class into the codebase under `service/achievement/` and run the tests.
