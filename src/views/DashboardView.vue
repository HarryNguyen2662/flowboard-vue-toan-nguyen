<script setup>
import { computed, ref, watch } from 'vue'
import TaskForm from '../components/TaskForm.vue'
import TaskItem from '../components/TaskItem.vue'
import TaskStats from '../components/TaskStats.vue'

const STORAGE_KEY = 'task-dashboard-items'

function loadTasks() {
  try {
    const saved = localStorage.getItem(STORAGE_KEY)
    if (!saved) {
      return []
    }

    const parsed = JSON.parse(saved)
    if (!Array.isArray(parsed)) {
      return []
    }

    return parsed
  } catch (error) {
    console.error('Unable to load tasks from localStorage.', error)
    return []
  }
}

const tasks = ref(loadTasks())
const filter = ref('all')
const search = ref('')

const completedCount = computed(() =>
  tasks.value.filter((task) => task.completed).length,
)

const tasksByStatus = computed(() => {
  if (filter.value === 'active') {
    return tasks.value.filter((task) => !task.completed)
  }

  if (filter.value === 'completed') {
    return tasks.value.filter((task) => task.completed)
  }

  return tasks.value
})

const visibleTasks = computed(() => {
  const query = search.value.trim().toLowerCase()

  if (!query) {
    return tasksByStatus.value
  }

  return tasksByStatus.value.filter((task) => {
    const searchableText = `${task.title} ${task.category}`.toLowerCase()
    return searchableText.includes(query)
  })
})

watch(
  tasks,
  (newTasks) => {
    localStorage.setItem(STORAGE_KEY, JSON.stringify(newTasks))
  },
  { deep: true },
)

function addTask(payload) {
  tasks.value.unshift({
    id: crypto.randomUUID(),
    title: payload.title,
    priority: payload.priority,
    category: payload.category,
    completed: false,
  })
}

function toggleStatus(taskId) {
  const selected = tasks.value.find((task) => task.id === taskId)
  if (selected) {
    selected.completed = !selected.completed
  }
}

function deleteTask(taskId) {
  tasks.value = tasks.value.filter((task) => task.id !== taskId)
}
</script>

<template>
  <div class="dashboard-layout">
    <TaskForm @add-task="addTask" />

    <TaskStats :total="tasks.length" :completed="completedCount" />

    <section class="toolbar">
      <label>
        Search
        <input
          v-model="search"
          type="text"
          placeholder="Search by title or category"
        />
      </label>

      <div class="filters" role="group" aria-label="Task status filter">
        <button
          type="button"
          :class="{ active: filter === 'all' }"
          @click="filter = 'all'"
        >
          All
        </button>
        <button
          type="button"
          :class="{ active: filter === 'active' }"
          @click="filter = 'active'"
        >
          Active
        </button>
        <button
          type="button"
          :class="{ active: filter === 'completed' }"
          @click="filter = 'completed'"
        >
          Completed
        </button>
      </div>
    </section>

    <section class="task-list" aria-live="polite">
      <TaskItem
        v-for="task in visibleTasks"
        :key="task.id"
        :task="task"
        @toggle-status="toggleStatus"
        @delete-task="deleteTask"
      />

      <p v-if="visibleTasks.length === 0" class="empty-state">
        No tasks match your current filter/search.
      </p>
    </section>
  </div>
</template>

<style scoped>
.dashboard-layout {
  display: grid;
  gap: 0.9rem;
}

.toolbar {
  display: flex;
  flex-wrap: wrap;
  align-items: flex-end;
  gap: 0.7rem;
  padding: 0.8rem;
  border: 1px solid var(--surface-300);
  border-radius: 10px;
  background: var(--surface-100);
}

label {
  display: grid;
  gap: 0.35rem;
  font-size: 0.9rem;
  font-weight: 600;
  color: var(--ink-700);
}

input {
  border: 1px solid var(--surface-400);
  border-radius: 8px;
  padding: 0.45rem 0.65rem;
  min-width: 240px;
  font: inherit;
  background: var(--surface-100);
}

.filters {
  display: inline-flex;
  gap: 0.35rem;
  flex-wrap: wrap;
}

.filters button {
  border: 1px solid var(--surface-400);
  border-radius: 8px;
  padding: 0.4rem 0.7rem;
  background: var(--surface-100);
  font-weight: 700;
  color: var(--ink-700);
  cursor: pointer;
}

.filters button.active {
  border-color: var(--brand-500);
  color: var(--brand-700);
  background: var(--brand-100);
}

.task-list {
  display: grid;
  gap: 0.65rem;
}

.empty-state {
  padding: 0.8rem;
  border-radius: 10px;
  background: var(--surface-100);
  border: 1px dashed var(--surface-400);
  color: var(--ink-500);
  margin: 0;
}

@media (max-width: 620px) {
  input {
    min-width: 100%;
  }

  .toolbar {
    align-items: stretch;
  }
}
</style>
