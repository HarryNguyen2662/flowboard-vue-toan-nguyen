# Task Management Dashboard (Vue 3 + Vite)

A functional task management dashboard built with Vue 3 Composition API and Vue Router.
It focuses on reactive state management, component communication, and clean project structure.

## Repository

https://github.com/HarryNguyen2662/flowboard-vue-toan-nguyen

## What this app includes

- Task creation with `title`, `priority` (`High`, `Medium`, `Low`), and `category`.
- Reactive task list with instant add/delete updates.
- Toggle task completion with strike-through visual feedback.
- Computed filtering by status: `All`, `Active`, `Completed`.
- Computed search by title/category keyword.
- Persistent storage using `watch(..., { deep: true })` + `localStorage`.
- Component communication using `props` and `emits`.
- Bonus feature: per-task 25-minute Focus Mode (Pomodoro) timer.

## Routing

- `/` -> Main dashboard
- `/about` -> "Productivity Methodology" explanation page

## Codebase structure

```text
src/
  components/
    TaskForm.vue      # Input form, emits new task payload
    TaskItem.vue      # Single task row, emits toggle/delete actions + focus timer
    TaskStats.vue     # Summary metrics (total/completed/active/rate)
  views/
    DashboardView.vue # Main state owner, computed filters/search, watch persistence
    AboutView.vue     # Methodology content
  router/
    index.js          # Route definitions
  App.vue             # App shell + navigation
  main.js             # App bootstrap + router registration
```

## State and communication flow

- `DashboardView.vue` owns the main `tasks` array.
- `TaskForm.vue` emits `add-task` to parent.
- `TaskItem.vue` receives `task` via `props`, emits `toggle-status` and `delete-task`.
- `TaskStats.vue` receives totals via `props`.
- `visibleTasks` is computed from status filter + search query.
- A deep `watch` persists task changes to `localStorage`.

## Run locally

1. Install dependencies

```bash
npm install
```

2. Run development server

```bash
npm run dev
```

3. Build for production

```bash
npm run build
```

4. Preview production build (optional)

```bash
npm run preview
```
