# ADR-002: Use Vue 3 + Spring Boot for the TaskMaster Course Project

- **Tags:** taskmaster, architecture, stack

## Context

The Software Engineering course requires me to develop a software product across the semester. I chose TaskMaster — a gamified task management app — because the domain is familiar, the scope is realistic for one semester and it gives me a chance to deepen skills.

The decision is *which* stack to commit to. I have professional experience with several, and the temptation is to pick something new "to learn." But the goal of the course project is to ship a complete, well-engineered product — not to maximize novelty.

## Options Considered

1. **Vue 3 (Composition API) + Spring Boot + MySQL.** What I use at work. Highest velocity, lowest risk.
2. **React + Node.js (NestJS) + PostgreSQL.** Industry-popular, would broaden my profile, but I would lose 3–4 weeks to ramp-up.
3. **Vue 3 + Go (Gin) + PostgreSQL.** Compromise — keep the frontend familiar, learn Go on the backend.

## Decision

Option 1. Vue 3 with Composition API, Spring Boot 3.4.3, Spring Security, Spring Data JPA, MySQL.

## Rationale

- The course is graded on engineering quality and the depth of the report, not on stack novelty.
- I want this project to be **portfolio-grade**. Familiar tools mean I can spend my time on architecture, testing, and writing — not on fighting the framework.
- I will get my "learn something new" need met through the *engineering practices* introduced by the course (CI/CD, ADRs, formal testing strategy), not through the stack.

## Consequences

- **Positive:** Faster delivery. More time for documentation and the report. Lower risk of unfinished features.
- **Negative:** No new stack on my CV from this project. I will need to compensate with side projects or the next course.
- **Follow-up:** After the course ends, start a side project to address the gap.
