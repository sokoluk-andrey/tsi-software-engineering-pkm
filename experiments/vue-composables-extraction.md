# Experiment: Extracting TaskMaster's Timer Logic into a Composable

- **Tags:** vue, taskmaster, refactoring

## Hypothesis

The timer logic in TaskMaster will eventually need to be reused across at least three views: the task detail page, the calendar quick-action menu, and the dashboard widget. If I extract it into a `useTimer()` composable now, the three views will share the same behavior with no duplication and no mixin gymnastics.

## Method

1. Built a single inline timer inside the task detail component (~80 lines of `<script setup>`).
2. Extracted the reactive state, the start/pause/reset functions, and the lifecycle cleanup into `composables/useTimer.js`.
3. Imported it into the dashboard widget as a smoke test.

## Result

The composable is 42 lines. The task detail component dropped from ~80 lines of timer code to 6. The dashboard widget needed 4 lines to integrate it. Zero duplicated state.

One issue surfaced: the original `setInterval` handle was not being cleared on component unmount in the inline version. Bug was invisible in single-page testing — I only noticed because extraction forced me to think about lifecycle. The composable now uses `onUnmounted` to clean up.

## Takeaway

Extraction is not just about reuse — it is a **diagnostic tool**. The act of pulling logic out of a component reveals lifecycle bugs that "works on my machine" hides. I will treat this as a habit going forward: if a piece of logic touches `setInterval`, `setTimeout`, or any subscription, it goes into a composable from day one.

## Follow-up

- Add a unit test for `useTimer` covering the unmount-cleanup case.
- Apply the same pattern to drag-and-drop logic next week.
