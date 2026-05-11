<script setup></script>

<template>
  <div></div>
</template>

<style scoped>
.stats-bar {
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 1rem;
  margin: 1rem 0 1.4rem;
  padding: 1rem 1.1rem;
  border-radius: 22px;
  border: 1px solid rgba(128, 93, 62, 0.1);
  background: rgba(255, 251, 245, 0.9);
  color: #5e4a3b;
  box-shadow: 0 14px 28px rgba(79, 50, 26, 0.06);
}

.stats-bar strong {
  color: #2f2218;
}

.stats-bar .muted {
  color: #8d7666;
  font-size: 0.94rem;
}

@media (max-width: 700px) {
  .stats-bar {
    flex-direction: column;
    align-items: flex-start;
  }
}
</style>
