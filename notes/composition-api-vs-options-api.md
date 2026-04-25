---
title: Vue 3 — Composition API vs Options API
tags: vue, frontend
---

# Composition API vs Options API

Both APIs produce the same runtime behavior in Vue 3. The difference is in how component logic is organized.

## Options API

Logic is grouped by *type*: `data`, `methods`, `computed`, `watch`, `mounted`. Reading a component means jumping between these blocks to follow a single feature.

This works well for small components. It scales badly when one component has multiple concerns — a feature ends up scattered across four sections of the file.

## Composition API

Logic is grouped by *concern* inside `setup()` (or `<script setup>`). All the state, computed values, and lifecycle hooks for one feature live next to each other. Features can be extracted into composables (`useTimer()`, `useDragAndDrop()`) and reused.

The trade-off is that the bar for "do I really know what `ref`, `reactive`, and `computed` actually do?" is higher. The Options API hides reactivity; the Composition API makes you think about it.

## Why I prefer Composition API for TaskMaster

TaskMaster has features (timers, drag-and-drop, achievements) that will be reused across multiple views. Composables map cleanly to that. With Options API I would either duplicate logic or reach for mixins, and mixins are exactly the problem the Composition API was designed to solve.

## Related
- `decisions/taskmaster-stack.md`
