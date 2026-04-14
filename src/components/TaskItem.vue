<script setup>
import { computed, onBeforeUnmount, ref } from 'vue'

const FOCUS_DURATION = 25 * 60

const props = defineProps({
  task: {
    type: Object,
    required: true,
  },
})

const emit = defineEmits(['toggle-status', 'delete-task'])

const secondsLeft = ref(FOCUS_DURATION)
const isFocusActive = ref(false)
let timerId = null

const priorityClass = computed(() => props.task.priority.toLowerCase())

const formattedFocusTime = computed(() => {
  const minutes = Math.floor(secondsLeft.value / 60)
  const seconds = secondsLeft.value % 60
  return `${String(minutes).padStart(2, '0')}:${String(seconds).padStart(2, '0')}`
})

function clearFocusTimer() {
  if (timerId) {
    clearInterval(timerId)
    timerId = null
  }
  isFocusActive.value = false
}

function startFocusTimer() {
  if (isFocusActive.value) {
    return
  }

  secondsLeft.value = FOCUS_DURATION
  isFocusActive.value = true

  timerId = setInterval(() => {
    if (secondsLeft.value <= 1) {
      clearFocusTimer()
      secondsLeft.value = 0
      return
    }

    secondsLeft.value -= 1
  }, 1000)
}

function resetFocusTimer() {
  clearFocusTimer()
  secondsLeft.value = FOCUS_DURATION
}

onBeforeUnmount(() => {
  clearFocusTimer()
})
</script>

<template>
  <article class="task-card">
    <div class="main-info">
      <span class="priority" :class="priorityClass">{{ task.priority }}</span>

      <div>
        <h3 :class="{ done: task.completed }">{{ task.title }}</h3>
        <p>{{ task.category }}</p>
      </div>
    </div>

    <div class="controls">
      <button type="button" class="secondary" @click="emit('toggle-status', task.id)">
        {{ task.completed ? 'Undo' : 'Complete' }}
      </button>

      <button type="button" class="danger" @click="emit('delete-task', task.id)">
        Delete
      </button>

      <button
        type="button"
        class="primary"
        :disabled="isFocusActive"
        @click="startFocusTimer"
      >
        {{ isFocusActive ? 'Focus Running' : 'Start Focus' }}
      </button>

      <button type="button" class="secondary" @click="resetFocusTimer">
        Reset {{ formattedFocusTime }}
      </button>
    </div>
  </article>
</template>

<style scoped>
.task-card {
  border: 1px solid var(--surface-300);
  border-radius: 10px;
  padding: 0.8rem;
  display: grid;
  gap: 0.7rem;
  background: var(--surface-100);
}

.main-info {
  display: flex;
  gap: 0.65rem;
  align-items: flex-start;
}

.priority {
  border-radius: 6px;
  padding: 0.2rem 0.45rem;
  font-size: 0.75rem;
  font-weight: 700;
  min-width: 56px;
  text-align: center;
}

.priority.high {
  background: #ffe4e6;
  color: #9f1239;
}

.priority.medium {
  background: #fef3c7;
  color: #92400e;
}

.priority.low {
  background: #dcfce7;
  color: #166534;
}

h3 {
  margin: 0;
  font-size: 1rem;
}

h3.done {
  text-decoration: line-through;
  color: var(--ink-500);
}

p {
  margin: 0.2rem 0 0;
  color: var(--ink-500);
  font-size: 0.9rem;
}

.controls {
  display: flex;
  flex-wrap: wrap;
  gap: 0.45rem;
}

button {
  border-radius: 8px;
  border: 1px solid var(--surface-400);
  padding: 0.4rem 0.6rem;
  font: inherit;
  font-size: 0.82rem;
  font-weight: 700;
  cursor: pointer;
}

.primary {
  border-color: var(--brand-700);
  background: var(--brand-700);
  color: white;
}

.primary:disabled {
  opacity: 0.6;
  cursor: not-allowed;
}

.secondary {
  background: var(--surface-100);
  color: var(--ink-700);
}

.danger {
  border-color: #dc2626;
  background: #dc2626;
  color: white;
}

@media (max-width: 620px) {
  .controls button {
    flex: 1 1 auto;
  }
}
</style>
