<script setup>
import { ref } from 'vue'

const emit = defineEmits(['add-task'])

const title = ref('')
const priority = ref('Medium')
const category = ref('')

function submitTask() {
  const cleanTitle = title.value.trim()
  const cleanCategory = category.value.trim()

  if (!cleanTitle || !cleanCategory) {
    return
  }

  emit('add-task', {
    title: cleanTitle,
    priority: priority.value,
    category: cleanCategory,
  })

  title.value = ''
  priority.value = 'Medium'
  category.value = ''
}
</script>

<template>
  <form class="task-form" @submit.prevent="submitTask">
    <h2>Add Task</h2>

    <div class="grid">
      <label>
        Title
        <input
          v-model="title"
          type="text"
          name="title"
          placeholder="Write client proposal"
          required
        />
      </label>

      <label>
        Category
        <input
          v-model="category"
          type="text"
          name="category"
          placeholder="Work, Study, Health..."
          required
        />
      </label>

      <label>
        Priority
        <select v-model="priority" name="priority">
          <option value="High">High</option>
          <option value="Medium">Medium</option>
          <option value="Low">Low</option>
        </select>
      </label>
    </div>

    <button type="submit">Add</button>
  </form>
</template>

<style scoped>
.task-form {
  padding: 0.9rem;
  border-radius: 10px;
  background: var(--surface-100);
  border: 1px solid var(--surface-300);
}

h2 {
  margin: 0 0 0.7rem;
  font-size: 1rem;
}

.grid {
  display: grid;
  gap: 0.65rem;
  grid-template-columns: repeat(auto-fit, minmax(180px, 1fr));
}

label {
  display: grid;
  gap: 0.35rem;
  font-weight: 600;
  font-size: 0.9rem;
  color: var(--ink-700);
}

input,
select {
  border: 1px solid var(--surface-400);
  border-radius: 8px;
  padding: 0.5rem 0.6rem;
  font: inherit;
  color: var(--ink-900);
  background: var(--surface-100);
}

input:focus,
select:focus {
  outline: none;
  border-color: var(--brand-500);
  box-shadow: 0 0 0 2px rgba(43, 103, 207, 0.15);
}

button {
  margin-top: 0.75rem;
  border: 0;
  border-radius: 8px;
  padding: 0.5rem 0.9rem;
  font-weight: 700;
  color: white;
  background: var(--brand-700);
  cursor: pointer;
}

button:hover {
  background: #173c7f;
}
</style>
