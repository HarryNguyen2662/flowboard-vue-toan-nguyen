<script setup>
import { computed } from 'vue'

const props = defineProps({
  total: {
    type: Number,
    required: true,
  },
  completed: {
    type: Number,
    required: true,
  },
})

const active = computed(() => props.total - props.completed)
const completionRate = computed(() => {
  if (props.total === 0) {
    return 0
  }
  return Math.round((props.completed / props.total) * 100)
})
</script>

<template>
  <section class="stats-grid">
    <article>
      <h3>Total</h3>
      <p>{{ total }}</p>
    </article>

    <article>
      <h3>Completed</h3>
      <p>{{ completed }}</p>
    </article>

    <article>
      <h3>Active</h3>
      <p>{{ active }}</p>
    </article>

    <article>
      <h3>Rate</h3>
      <p>{{ completionRate }}%</p>
    </article>
  </section>
</template>

<style scoped>
.stats-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(130px, 1fr));
  gap: 0.6rem;
}

article {
  padding: 0.65rem 0.75rem;
  border-radius: 10px;
  border: 1px solid var(--surface-300);
  background: var(--surface-100);
}

h3 {
  margin: 0;
  font-size: 0.75rem;
  color: var(--ink-500);
  text-transform: uppercase;
  letter-spacing: 0.05em;
}

p {
  margin: 0.2rem 0 0;
  font-size: 1.3rem;
  font-weight: 700;
  color: var(--ink-900);
}
</style>
