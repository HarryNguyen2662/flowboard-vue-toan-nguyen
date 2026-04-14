<template>
  <div class="app-shell">
    <header class="topbar">
      <div class="title-wrap">
        <p class="eyebrow">Task Manager</p>
        <h1>FlowBoard</h1>
      </div>
      <nav class="nav-links">
        <RouterLink to="/">Dashboard</RouterLink>
        <RouterLink to="/about">About</RouterLink>
      </nav>
    </header>

    <main>
      <RouterView />
    </main>
  </div>
</template>

<style scoped>
.app-shell {
  max-width: 920px;
  margin: 0 auto;
  padding: 1.2rem 1rem 2rem;
}

.topbar {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
  gap: 0.9rem;
  align-items: end;
  margin-bottom: 1.25rem;
}

.title-wrap {
  display: grid;
  gap: 0.2rem;
}

.eyebrow {
  margin: 0;
  font-size: 0.78rem;
  color: var(--ink-300);
  letter-spacing: 0.06em;
  text-transform: uppercase;
  font-weight: 600;
}

h1 {
  font-size: clamp(1.4rem, 4vw, 1.9rem);
}

.nav-links {
  display: inline-flex;
  gap: 0.45rem;
  flex-wrap: wrap;
}

.nav-links a {
  text-decoration: none;
  font-weight: 600;
  color: var(--ink-700);
  border: 1px solid var(--surface-400);
  border-radius: 8px;
  padding: 0.4rem 0.75rem;
  background: var(--surface-100);
}

.nav-links a:hover {
  border-color: var(--brand-500);
  color: var(--brand-700);
}

.nav-links :global(.router-link-active) {
  border-color: var(--brand-500);
  color: var(--brand-700);
  background: var(--brand-100);
}
</style>
