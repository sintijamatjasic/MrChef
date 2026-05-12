<script setup>
import RecipePage from './pages/RecipePage.vue'
</script>

<template>
  <RecipePage />
</template>

<style>
* {
  box-sizing: border-box;
}

html {
  scroll-behavior: smooth;
}

body {
  margin: 0;
  font-family:
    Inter,
    ui-sans-serif,
    system-ui,
    -apple-system,
    BlinkMacSystemFont,
    'Segoe UI',
    sans-serif;
  background: #f7f2ea;
  color: #2b2118;
}

button,
input,
select {
  font: inherit;
}

img {
  display: block;
  max-width: 100%;
}

#app {
  min-height: 100vh;
}

::-webkit-scrollbar {
  width: 10px;
}

::-webkit-scrollbar-track {
  background: #f4ece2;
}

::-webkit-scrollbar-thumb {
  background: #c8ab90;
  border-radius: 999px;
}

::-webkit-scrollbar-thumb:hover {
  background: #b48f70;
}
</style>
