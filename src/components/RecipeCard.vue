<script setup>
defineProps({
  recipe: Object,
})
</script>

<template>
  <div class="recipe-card">
    <article class="recipe-card">
      <img :src="recipe.image" :alt="recipe.name" class="recipe-image" />

      <div class="recipe-content">
        <div class="recipe-top">
          <h3 class="recipe-title">{{ recipe.name }}</h3>
          <span class="recipe-type">{{ recipe.type }}</span>
        </div>

        <div class="recipe-meta">
          <span><i class="fa-regular fa-clock"></i> {{ recipe.time }} min</span>
          <span><i class="fa-solid fa-star yellow"></i> {{ recipe.rating }} rating</span>
          <span>{{ recipe.difficulty }}</span>
          <span>{{ recipe.servings }} servings</span>
        </div>

        <div class="recipe-bottom">
          <span></span>

          <button
            @click="$emit('toggle-favorite', recipe.id)"
            class="favorite-btn"
            :class="{ active: recipe.favorite }"
          >
            <i :class="recipe.favorite ? 'fa-solid fa-heart' : 'fa-regular fa-heart'"></i>
          </button>
        </div>
      </div>
    </article>
  </div>
</template>

<style scoped>
.recipe-card {
  overflow: hidden;
  border-radius: 26px;
  background: #fffdf9;
  border: 1px solid rgba(128, 93, 62, 0.08);
  box-shadow:
    0 18px 34px rgba(79, 50, 26, 0.08),
    inset 0 1px 0 rgba(255, 255, 255, 0.6);
  transition:
    transform 0.22s ease,
    box-shadow 0.22s ease;
}

.yellow {
  color: rgb(255, 214, 33);
}

.recipe-card:hover {
  transform: translateY(-4px);
  box-shadow: 0 24px 40px rgba(79, 50, 26, 0.12);
}

.recipe-image {
  width: 100%;
  aspect-ratio: 4 / 3;
  object-fit: cover;
  background: #f0e5d8;
}

.recipe-content {
  flex: 1;
  display: flex;
  flex-direction: column;
  padding: 0.9rem;
}

.recipe-top {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  gap: 1rem;
}

.recipe-title {
  margin: 0;
  font-size: 1.18rem;
  line-height: 1.2;
  font-weight: 800;
  letter-spacing: -0.02em;
  color: #2f2218;
  min-height: 2.8rem;
  margin-bottom: 0.5rem;
}

.recipe-type {
  display: inline-flex;
  align-items: center;
  padding: 0.34rem 0.68rem;
  border-radius: 999px;
  background: #f3e4d4;
  color: #9b633a;
  font-size: 0.76rem;
  font-weight: 700;
  letter-spacing: 0.03em;
  white-space: nowrap;
}

.recipe-meta {
  display: flex;
  flex-wrap: wrap;
  gap: 0.5rem 0.65rem;
  color: #846d5c;
  font-size: 0.9rem;
}

.recipe-meta span {
  padding: 0.38rem 0.62rem;
  border-radius: 999px;
  background: #f8f2ea;
}

.recipe-bottom {
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 0.75rem;
  margin-top: 0.2rem;
}

.favorite-btn {
  width: 42px;
  height: 42px;
  border: 1px solid rgba(128, 93, 62, 0.1);
  border-radius: 14px;
  background: #fff8f1;
  color: #b18b74;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  transition: all 0.2s ease;
}

.favorite-btn:hover {
  color: #d07d45;
  transform: translateY(-1px);
}

.favorite-btn.active {
  background: linear-gradient(180deg, #d98c53, #c17139);
  color: white;
  border-color: transparent;
  box-shadow: 0 10px 20px rgba(193, 113, 57, 0.18);
}
</style>
