<script setup>
import { ref, computed } from 'vue'

const { recipe } = defineProps({
  recipe: Object,
})

defineEmits(['close'])

const selectedServings = ref(recipe.servings)

function decreaseServings() {
  if (selectedServings.value > 1) {
    selectedServings.value--
  }
}

function increaseServings() {
  selectedServings.value++
}

const scaledIngredients = computed(() => {
  return recipe.ingredients.map((ingredient) => {
    const scaledAmount = (ingredient.amount / recipe.servings) * selectedServings.value

    return {
      ...ingredient,
      scaledAmount,
    }
  })
})
</script>

<template>
  <div class="modal-backdrop" @click="$emit('close')">
    <div class="modal-card" @click.stop>
      <button class="modal-close" @click="$emit('close')">
        <i class="fa-solid fa-xmark"></i>
      </button>

      <img :src="recipe.image" :alt="recipe.name" class="modal-image" />

      <div class="modal-content">
        <div class="modal-header">
          <div class="modal-heading">
            <span class="modal-type">{{ recipe.type }}</span>
            <h2>{{ recipe.name }}</h2>
            <p class="modal-description">{{ recipe.description }}</p>
          </div>

          <div class="modal-meta">
            <span><i class="fa-regular fa-clock"></i> {{ recipe.time }} min</span>
            <span><i class="fa-solid fa-star yellow"></i> {{ recipe.rating }}</span>
            <span>{{ recipe.difficulty }}</span>
            <span
              ><strong>{{ selectedServings }}</strong> servings</span
            >
          </div>
        </div>

        <div class="servings-control">
          <h4>Adjust servings</h4>

          <div class="servings-box">
            <button class="servings-btn" @click="decreaseServings">-</button>
            <span class="servings-value">{{ selectedServings }}</span>
            <button class="servings-btn" @click="increaseServings">+</button>
          </div>
        </div>

        <div class="modal-grid">
          <section class="modal-section">
            <h4>Ingredients</h4>
            <ul class="ingredients-list">
              <li v-for="ingredient in scaledIngredients" :key="ingredient.name">
                <strong>{{ ingredient.scaledAmount }} {{ ingredient.unit }}</strong>
                {{ ingredient.name }}
              </li>
            </ul>
          </section>

          <section class="modal-section">
            <h4>Instructions</h4>
            <ol class="instructions-list">
              <li v-for="instruction in recipe.instructions" :key="instruction">
                {{ instruction }}
              </li>
            </ol>
          </section>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.modal-backdrop {
  position: fixed;
  inset: 0;
  background: rgba(34, 23, 15, 0.55);
  backdrop-filter: blur(6px);
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 1.25rem;
  z-index: 1000;
}

.modal-card {
  position: relative;
  width: min(960px, 100%);
  max-height: 90vh;
  overflow: auto;
  background: #fffdf9;
  border-radius: 28px;
  box-shadow: 0 30px 80px rgba(47, 34, 24, 0.22);
  border: 1px solid rgba(128, 93, 62, 0.08);
  scrollbar-width: thin;
  scrollbar-color: #c8ab90 #f7efe5;
}

.modal-card::-webkit-scrollbar {
  width: 10px;
}

.modal-card::-webkit-scrollbar-track {
  background: #f7efe5;
  border-radius: 999px;
}

.modal-card::-webkit-scrollbar-thumb {
  background: #c8ab90;
  border-radius: 999px;
}

.modal-card::-webkit-scrollbar-thumb:hover {
  background: #b48f70;
}

.modal-close {
  position: absolute;
  top: 1rem;
  right: 1rem;
  width: 42px;
  height: 42px;
  border: none;
  border-radius: 14px;
  background: rgba(255, 255, 255, 0.92);
  color: #6a5646;
  cursor: pointer;
  box-shadow: 0 8px 18px rgba(79, 50, 26, 0.08);
  z-index: 2;
  transition: all 0.2s ease;
}

.modal-close:hover {
  transform: translateY(-1px);
  color: #2f2218;
}

.modal-image {
  width: 100%;
  height: 320px;
  object-fit: cover;
}

.modal-content {
  padding: 1.5rem 1.7rem 1.5rem 1.5rem;
  display: flex;
  flex-direction: column;
  gap: 1.4rem;
}

.modal-header {
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

.modal-heading {
  display: flex;
  flex-direction: column;
  gap: 0.75rem;
}

.modal-type {
  display: inline-flex;
  align-items: center;
  align-self: flex-start;
  padding: 0.42rem 0.8rem;
  border-radius: 999px;
  background: #f3e4d4;
  color: #9b633a;
  font-size: 0.8rem;
  font-weight: 700;
  white-space: nowrap;
}

.modal-heading h2 {
  margin: 0;
  font-family: Georgia, 'Times New Roman', serif;
  font-size: 2.1rem;
  line-height: 1.05;
  color: #2f2218;
  letter-spacing: -0.03em;
}

.modal-description {
  margin: 0;
  color: #6c5645;
  line-height: 1.8;
  max-width: 65ch;
}

.modal-meta {
  display: flex;
  flex-wrap: wrap;
  gap: 0.65rem;
  color: #846d5c;
  font-size: 0.95rem;
}

.modal-meta span {
  padding: 0.45rem 0.75rem;
  border-radius: 999px;
  background: #f8f2ea;
  display: inline-flex;
  align-items: center;
  gap: 0.4rem;
}

.yellow {
  color: rgb(255, 214, 33);
}

.modal-grid {
  display: grid;
  grid-template-columns: 0.9fr 1.1fr;
  gap: 1.25rem;
  align-items: start;
}

.modal-section {
  display: flex;
  flex-direction: column;
  gap: 0.85rem;
  padding: 1rem;
  border-radius: 22px;
  background: #fcf7f0;
  border: 1px solid rgba(128, 93, 62, 0.08);
}

.modal-section h4 {
  margin: 0;
  font-size: 1rem;
  font-weight: 800;
  color: #2f2218;
  letter-spacing: -0.01em;
}

.ingredients-list {
  margin: 0;
  padding-left: 1.2rem;
  display: flex;
  flex-direction: column;
  gap: 0.55rem;
  color: #6c5645;
}

.ingredients-list li {
  line-height: 1.6;
}

.servings-control {
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 1rem;
  padding: 1rem;
  border-radius: 20px;
  background: #fcf7f0;
  border: 1px solid rgba(128, 93, 62, 0.08);
}

.servings-control h4 {
  margin: 0;
  font-size: 1rem;
  font-weight: 800;
  color: #2f2218;
}

.servings-box {
  display: flex;
  align-items: center;
  gap: 0.75rem;
}

.servings-btn {
  width: 38px;
  height: 38px;
  border: 1px solid rgba(128, 93, 62, 0.12);
  border-radius: 12px;
  background: #fffdf9;
  color: #6a5646;
  font-size: 1.2rem;
  font-weight: 700;
  cursor: pointer;
  transition: all 0.2s ease;
}

.servings-btn:hover {
  transform: translateY(-1px);
  border-color: rgba(153, 98, 58, 0.28);
  color: #2f2218;
}

.servings-value {
  min-width: 2rem;
  text-align: center;
  font-size: 1.05rem;
  font-weight: 700;
  color: #2f2218;
}

.instructions-list {
  margin: 0;
  padding-left: 1.2rem;
  display: flex;
  flex-direction: column;
  gap: 0.8rem;
  color: #6c5645;
}

.instructions-list li {
  line-height: 1.75;
  padding-left: 0.15rem;
}

.instructions-list li::marker {
  font-weight: bold;
}

@media (max-width: 800px) {
  .modal-image {
    height: 240px;
  }

  .modal-grid {
    grid-template-columns: 1fr;
  }

  .modal-heading h2 {
    font-size: 1.7rem;
  }
}
</style>
