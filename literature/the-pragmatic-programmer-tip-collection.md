---
title: The Pragmatic Programmer — Tips That Hit Differently as a Team Lead
source: "Hunt, A. & Thomas, D. (2019). The Pragmatic Programmer, 20th Anniversary Edition."
tags: literature, professional-practice
---

# The Pragmatic Programmer — Re-read as a Team Lead

I read this book the first time when I was 16, self-taught, with no professional context. Most of it bounced off. Trying to fit it into my current schedule and re-read it now, three weeks before stepping into a Team Lead role, the same sentences mean different things.

## What the authors say

> *(Paraphrased)* The book frames software development as a craft, not a job. It argues that responsibility for one's work — including for understanding *why* requirements exist — is what separates a programmer from a coder. It treats "DRY" as a principle about knowledge representation, not just code duplication. It presents broken-windows theory as applied to codebases: small visible decay invites larger decay.

## What I think about it

What landed differently this time:

- **DRY is not about code.** At 16 I read it as "don't copy-paste." At 22, leading a small team, I see it as a knowledge-management principle. Two people on my team holding incompatible mental models of the same business rule is a DRY violation, even if no code is duplicated. ADRs are a DRY tool.

- **"Good-enough software" is not a compromise, it is an engineering decision.** This was the chapter I most resisted at 17. Now I understand that a system polished beyond what the user needs is just as much a failure as one shipped under-baked. The skill is calibrating "enough" — and that calibration requires talking to users, which juniors usually don't do.

- **Broken windows applies to teams, not just code.** A junior who sees lazy reviews learns that lazy reviews are normal. Whatever I let slide as Team Lead becomes the team's floor, not its ceiling.

## What I disagree with or would update

The book is mostly stack-agnostic, but where it touches tooling (chapter on text manipulation, on shells) it is showing its age. The deeper principles hold; the specific tool recommendations don't. Not a flaw — just a reminder that the half-life of advice scales with its abstraction level.

## What I will do

- Write the team's first ADR within the first month as Team Lead.
- Push back at least once when a "we'll fix it later" decision is made without a follow-up ticket. That is the broken window.

## Related

- `notes/what-makes-a-good-adr.md`
