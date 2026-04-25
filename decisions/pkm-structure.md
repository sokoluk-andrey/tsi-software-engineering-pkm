# ADR-001: Adopt a Cognitive PKM Structure Based on Folders + Git

- **Tags:** meta, knowledge-management

## Context

I have been keeping notes for years — across Notion, plain text files, and the margins of textbooks — but the system has never survived more than a few months. The reason, I think, is that every previous attempt optimized for **capture** instead of **thinking**. I ended up with archives I never returned to.

Now I am 3rd year student at TSI, who works as a Middle Full-Stack developer, transitioning into a Team Lead role at ClarityLabs and aiming for Senior in two years. I cannot afford another system that just collects.

## Options Considered

1. **Notion / Obsidian with a tag-based structure.** Excellent UX, weak versioning. The history of my thinking is invisible.
2. **A single long Markdown journal.** Simple, but no separation between transient notes and durable knowledge. Decisions get buried.
3. **Folder-based PKM in Git, modeled after the course's illustrative example.** Forces structure, gives me a real history of how my understanding evolved, and integrates with the tools I already use daily.

## Decision

Option 3. A Git repository with separate folders for `decisions/`, `notes/`, `experiments/`, `literature/` and `questions/`. Rules defined in the README.

## Consequences

- **Positive:** Decisions become traceable. I can `git log` my own thinking. The repo doubles as evidence of growth for the course and for my career.
- **Negative:** Higher friction than tag-based tools. No mobile-friendly editor. I have to commit deliberately, which I expect to fail at sometimes.
- **Mitigation:** Weekly review on Sundays. If after one month I am not committing at least 3 times a week, I will reduce the structure rather than abandon it.
