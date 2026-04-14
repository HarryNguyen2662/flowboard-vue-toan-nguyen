<template>
  <article class="about">
    <h2>Productivity Methodology</h2>
    <p>
      This dashboard combines the Eisenhower Matrix mindset with a Pomodoro focus rhythm.
      Each task has a priority and category so you can decide what is urgent, important, or
      better deferred.
    </p>

    <h3>How It Works</h3>
    <ul>
      <li>
        Prioritize quickly using High, Medium, and Low tags to avoid decision fatigue.
      </li>
      <li>
        Keep momentum by filtering between All, Active, and Completed tasks.
      </li>
      <li>
        Use Focus Mode on a task to run a dedicated 25-minute work sprint.
      </li>
      <li>
        Review completion stats to plan your next batch realistically.
      </li>
    </ul>

    <p class="note">
      The app also saves tasks in your browser storage, so your workflow survives refreshes.
    </p>
  </article>
</template>

<style scoped>
.about {
  border: 1px solid var(--surface-300);
  border-radius: 10px;
  padding: 1rem;
  background: var(--surface-100);
}

h3 {
  margin-top: 1rem;
}

p,
ul {
  color: var(--ink-700);
}

ul {
  margin: 0;
  padding-left: 1.1rem;
  display: grid;
  gap: 0.35rem;
}

.note {
  margin-top: 1rem;
  border-radius: 8px;
  background: var(--surface-200);
  border: 1px solid var(--surface-300);
  padding: 0.65rem;
}
</style>
