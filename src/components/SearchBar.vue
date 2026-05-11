<script setup>
const model = defineModel({ default: '' })
</script>

<template>
  <div class="search-bar">
    <label for="search"><i class="fa-solid fa-magnifying-glass"></i></label>
    <input type="text" placeholder="Find you next meal..." v-model="model" />
  </div>
</template>

<style scoped>
.search-bar {
  width: 100%;
  position: relative;
}

.search-bar input {
  width: 100%;
  height: 58px;
  padding: 0.95rem 1rem 0.95rem 3rem;
  border-radius: 18px;
  border: 1px solid rgba(128, 93, 62, 0.12);
  background: #fffdf9;
  color: #2f2218;
  outline: none;
  box-shadow:
    inset 0 1px 0 rgba(255, 255, 255, 0.7),
    0 12px 24px rgba(79, 50, 26, 0.06);
  transition: all 0.2s ease;
}

.search-bar input::placeholder {
  color: #aa9482;
}

.search-bar input:focus {
  border-color: rgba(153, 98, 58, 0.45);
  box-shadow:
    0 0 0 4px rgba(153, 98, 58, 0.12),
    0 12px 24px rgba(79, 50, 26, 0.08);
}

.search-bar label {
  position: absolute;
  top: 50%;
  left: 1rem;
  transform: translateY(-50%);
  color: #a36a41;
  pointer-events: none;
  font-size: 0.95rem;
}
</style>
