---
title: Vue 3 — Composition API vs Options API
tags: vue, frontend
---

# Composition API vs Options API

Both APIs produce the same runtime behavior in Vue 3. The difference is in how component logic is **organized**.

## Options API

Logic is grouped by *type*: `data`, `methods`, `computed`, `watch`, `mounted`. Reading a component means jumping between these blocks to follow a single feature.

This works well for small components. It scales badly when one component has multiple concerns — a feature ends up scattered across four sections of the file.

## Composition API

Logic is grouped by *concern* inside `setup()` (or `<script setup>`). All the state, computed values, and lifecycle hooks for one feature live next to each other. Features can be extracted into composables (`useTimer()`, `useDragAndDrop()`) and reused.

The trade-off is that the bar for "do I really know what `ref`, `reactive`, and `computed` actually do?" is higher. The Options API hides reactivity; the Composition API makes you think about it.

## Why I prefer Composition API for TaskMaster

TaskMaster has features (timers, drag-and-drop, achievements) that will be reused across multiple views. Composables map cleanly to that. With Options API I would either duplicate logic or reach for mixins, and mixins are exactly the problem the Composition API was designed to solve.


---
title: JWT vs Session Authentication — When Each Wins
tags: security, backend, auth
---

# JWT vs Session Authentication

The internet's answer to "JWT or sessions?" is mostly tribal. The honest answer is: it depends on what your system actually needs.

## Sessions

The server stores session state (typically in Redis or a database). The client holds an opaque session ID in a cookie. To log a user out, you delete the session record. Simple, revocable, server-controlled.

**Cost:** every authenticated request hits the session store. At scale, this is real load.

## JWT

The server signs a token containing the user's claims. The client sends it back. The server verifies the signature without any database lookup. Stateless.

**Cost:** revocation is hard. A signed token is valid until it expires. To log someone out *immediately*, you need a blocklist — at which point you have reintroduced the state you used JWT to avoid.

## What I am using for TaskMaster, and why

JWT with a short access-token lifetime (15 min) and a longer-lived refresh token. For a course project with a single backend, sessions would actually be simpler. But:

- I want hands-on practice with the patterns I will face at work, where services are split.
- The pain of refresh-token rotation is itself something worth learning.
- If I add a mobile client later, JWT scales to that without redesign.

## What I'd reconsider

For a banking app or anything with a real "log them out *now*" requirement, I would use sessions. JWT's statelessness is a feature only when you don't need fine-grained revocation.


---
title: What Makes a Good ADR
tags: process, decisions, writing
---

# What Makes a Good ADR

An Architectural Decision Record is not a design document. It is a record of a *moment of choice* — what was on the table, what I picked, and why.

## The four sections that matter

1. **Context.** What was the situation? What forced the decision? Without this, future-me cannot tell whether the decision still applies.
2. **Options.** What alternatives were considered? Listing only the chosen option hides the reasoning.
3. **Decision.** What was chosen. One sentence, ideally.
4. **Consequences.** What this commits me to — both good and bad. The bad part is the most important and the most often skipped.

## Common failure modes

- **Writing the ADR after the decision is already implemented.** It becomes a justification, not a record. The "Options" section becomes fiction.
- **Leaving out the "bad" consequences.** A decision with no downsides is a decision that was not actually examined.
- **Editing accepted ADRs.** Once accepted, an ADR is immutable. If the decision changes, a new ADR supersedes it. Otherwise, the history lies.

## Test for whether it's worth an ADR

Ask: *"In six months, if someone (including me) reverses this decision, would they need to know why it was made?"* If yes — write the ADR. If no — it's just an implementation detail.
