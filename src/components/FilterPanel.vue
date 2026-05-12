<script setup>
defineProps({
  cookingTimes: Array,
  minRatings: Array,
  sortOptions: Array,
  mealTypes: Array,
})

const selectedTime = defineModel('selectedTime', { default: 'All' })
const selectedRating = defineModel('selectedRating', { default: 0 })
const selectedSort = defineModel('selected-sort', { default: 'Default' })
const selectedType = defineModel('selected-type', { default: 'All' })
</script>

<template>
  <div class="filter-panel">
    <div class="filter-group">
      <p class="filter-label">Cooking time</p>

      <div class="filter-buttons">
        <button
          v-for="cookingTime in cookingTimes"
          :key="cookingTime"
          type="button"
          class="filter-btn"
          :class="{ active: selectedTime === cookingTime }"
          @click="selectedTime = cookingTime"
        >
          {{ cookingTime }}
        </button>
      </div>
    </div>

    <div class="filter-group">
      <p class="filter-label">Meal Type</p>

      <div class="filter-buttons">
        <button
          v-for="mealType in mealTypes"
          :key="mealType"
          type="button"
          class="filter-btn"
          :class="{ active: selectedType === mealType }"
          @click="selectedType = mealType"
        >
          {{ mealType }}
        </button>
      </div>
    </div>

    <div class="filter-group">
      <p class="filter-label">Rating</p>

      <div class="filter-buttons">
        <button
          v-for="minRating in minRatings"
          :key="minRating"
          type="button"
          class="filter-btn"
          :class="{ active: selectedRating === minRating }"
          @click="selectedRating = minRating"
        >
          {{ minRating === 0 ? 'All' : `${minRating}+` }}
        </button>
      </div>
    </div>

    <div class="filter-group">
      <p class="filter-label">Sort by</p>

      <div class="filter-buttons">
        <button
          v-for="sortOption in sortOptions"
          :key="sortOption"
          type="button"
          class="filter-btn"
          :class="{ active: selectedSort === sortOption }"
          @click="selectedSort = sortOption"
        >
          {{ sortOption }}
        </button>
      </div>
    </div>
  </div>
</template>

<style scoped>
.filter-panel {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 1rem;
  width: 100%;
  align-items: start;
}

.filter-group {
  display: flex;
  flex-direction: column;
  gap: 0.55rem;
  min-width: 0;
  padding: 1rem;
  border-radius: 20px;
  background: rgba(255, 251, 245, 0.9);
  border: 1px solid rgba(128, 93, 62, 0.08);
  box-shadow: 0 10px 20px rgba(79, 50, 26, 0.04);
}

.filter-label {
  margin: 0;
  font-size: 0.78rem;
  font-weight: 700;
  letter-spacing: 0.06em;
  text-transform: uppercase;
  color: #8e7663;
}

.filter-buttons {
  display: flex;
  flex-wrap: wrap;
  gap: 0.65rem;
}

.filter-btn {
  border: 1px solid rgba(128, 93, 62, 0.12);
  background: #fffdf9;
  color: #6a5646;
  padding: 0.72rem 1rem;
  border-radius: 999px;
  font-size: 0.92rem;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.2s ease;
  box-shadow: 0 10px 18px rgba(79, 50, 26, 0.05);
}

.filter-btn:hover {
  transform: translateY(-1px);
  border-color: rgba(153, 98, 58, 0.28);
  color: #2f2218;
}

.filter-btn.active {
  background: linear-gradient(180deg, #c9834d, #b56d38);
  color: white;
  border-color: transparent;
  box-shadow: 0 12px 22px rgba(181, 109, 56, 0.18);
}

@media (max-width: 720px) {
  .filter-panel {
    grid-template-columns: 1fr;
  }
}
</style>
