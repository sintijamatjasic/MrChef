<script setup>
import RecipeCard from './RecipeCard.vue'

defineProps({
  recipes: Object,
})
</script>

<template>
  <div class="recipe-list">
    <RecipeCard
      v-for="recipe in recipes"
      key="recipe.id"
      :recipe="recipe"
      @toggle-favorite="$emit('toggle-favorite', $event)"
      @view-recipe="$emit('view-recipe', $event)"
    />
  </div>
</template>

<style scoped>
.recipe-list {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(260px, 1fr));
  gap: 1.25rem;
}

.empty-state {
  text-align: center;
  padding: 3rem 1.25rem;
  border-radius: 28px;
  border: 1px solid rgba(128, 93, 62, 0.1);
  background: rgba(255, 251, 245, 0.9);
  color: #8d7666;
  box-shadow: 0 14px 28px rgba(79, 50, 26, 0.06);
}

.empty-state h3 {
  margin: 0 0 0.65rem;
  color: #2f2218;
  font-size: 1.3rem;
}

.empty-state p {
  margin: 0;
  line-height: 1.6;
}
</style>
