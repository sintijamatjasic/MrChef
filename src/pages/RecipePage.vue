<script setup>
import FilterPanel from '@/components/FilterPanel.vue'
import RecipeList from '@/components/RecipeList.vue'
import RecipeModal from '@/components/RecipeModal.vue'
import SearchBar from '@/components/SearchBar.vue'
import StatsBar from '@/components/StatsBar.vue'
import { recipesData } from '@/data/data'
import { computed, onMounted, ref } from 'vue'

const recipes = ref(recipesData)
const searchValue = ref('')
const timeValue = ref('All')
const ratingValue = ref(0)
const favoritesOnly = ref(false)
const typeValue = ref('All')
const showFilters = ref(false)
const selectedRecipe = ref(null)

const cookingTimes = ['All', 'Under 15 min', 'Under 30 min', 'Under 45 min', '45+ min']
const minRatings = [0, 4, 4.5, 4.8]
const sortOptions = ['Default', 'Shortest time', 'Highest rating', 'A-Z']
const sortValue = ref('Default')
const mealTypes = ['All', 'Breakfast', 'Lunch', 'Dinner', 'Dessert']

const filteredRecipes = computed(() => {
  const filtered = recipes.value.filter((recipe) => {
    const matchesSearch = recipe.name.toLowerCase().includes(searchValue.value.toLowerCase())
    const matchesTime =
      timeValue.value === 'All' ||
      (timeValue.value === 'Under 15 min' && recipe.time < 15) ||
      (timeValue.value === 'Under 30 min' && recipe.time < 30) ||
      (timeValue.value === 'Under 45 min' && recipe.time < 45) ||
      (timeValue.value === '45+ min' && recipe.time >= 45)
    const matchesRating =
      ratingValue.value === 0 ||
      (ratingValue.value === 4 && recipe.rating >= 4) ||
      (ratingValue.value === 4.5 && recipe.rating >= 4.5) ||
      (ratingValue.value === 4.8 && recipe.rating >= 4.8)

    const matchesType = typeValue.value === 'All' || typeValue.value === recipe.type

    const matchesFavorite = !favoritesOnly.value || recipe.favorite
    return matchesSearch && matchesTime && matchesRating && matchesFavorite && matchesType
  })

  const sorted = [...filtered]

  if (sortValue.value === 'Shortest time') {
    sorted.sort((a, b) => a.time - b.time)
  } else if (sortValue.value === 'Highest rating') {
    sorted.sort((a, b) => b.rating - a.rating)
  } else if (sortValue.value === 'A-Z') {
    sorted.sort((a, b) => a.name.localeCompare(b.name))
  }

  return sorted
})

function toggleFavorite(recipeId) {
  const recipe = recipes.value.find((item) => item.id === recipeId)

  if (recipe) {
    recipe.favorite = !recipe.favorite
    saveFavoritesToStorage()
  }
}

function toggleFilters() {
  showFilters.value = !showFilters.value
}

function clearFilters() {
  searchValue.value = ''
  timeValue.value = 'All'
  ratingValue.value = 0
  favoritesOnly.value = false
  typeValue.value = 'All'
  sortValue.value = 'Default'
  showFilters.value = false
}

const hasActiveFilters = computed(() => {
  return (
    searchValue.value !== '' ||
    timeValue.value !== 'All' ||
    ratingValue.value !== 0 ||
    typeValue.value !== 'All' ||
    favoritesOnly.value
  )
})

function openRecipeModal(recipe) {
  selectedRecipe.value = recipe
}

function closeRecipeModal() {
  selectedRecipe.value = null
}

function saveFavoritesToStorage() {
  const favoriteIds = recipes.value.filter((recipe) => recipe.favorite).map((recipe) => recipe.id)

  localStorage.setItem('favoriteRecipes', JSON.stringify(favoriteIds))
}

function loadFavoritesFromStorage() {
  const storedFavorites = localStorage.getItem('favoriteRecipes')

  if (!storedFavorites) return

  const favoriteIds = JSON.parse(storedFavorites)

  recipes.value.forEach((recipe) => {
    recipe.favorite = favoriteIds.includes(recipe.id)
  })
}

onMounted(() => {
  loadFavoritesFromStorage()
})
</script>

<template>
  <div class="page">
    <section class="hero">
      <div class="hero-content">
        <h1>Seasonal recipes worth making</h1>
        <p>
          Browse cozy breakfasts, quick lunches, hearty dinners, and desserts that feel homemade and
          polished.
        </p>
      </div>

      <img
        class="hero-image"
        src="https://images.unsplash.com/photo-1547592180-85f173990554?auto=format&fit=crop&w=1200&q=80"
        alt="Food hero"
      />
    </section>
    <div class="controls">
      <div class="top-row">
        <SearchBar v-model="searchValue" />
        <button
          class="favorites-btn"
          :class="{ active: favoritesOnly }"
          @click="favoritesOnly = !favoritesOnly"
        >
          {{ favoritesOnly ? 'Showing favorites' : 'Favorites only' }}
        </button>
        <button v-if="hasActiveFilters" class="clear-btn" @click="clearFilters">
          Clear filters
        </button>
      </div>

      <button class="toggle-filters-btn" @click="toggleFilters">
        {{ showFilters ? 'Hide filters' : 'Show filters' }}
      </button>
      <transition name="filters-fade"
        ><FilterPanel
          v-if="showFilters"
          :cookingTimes="cookingTimes"
          :minRatings="minRatings"
          :sortOptions="sortOptions"
          :mealTypes="mealTypes"
          v-model:selected-time="timeValue"
          v-model:selected-rating="ratingValue"
          v-model:selected-sort="sortValue"
          v-model:selected-type="typeValue"
      /></transition>
    </div>

    <StatsBar
      :count="filteredRecipes.length"
      :selected-time="timeValue"
      :selected-rating="ratingValue"
    />

    <RecipeList
      :recipes="filteredRecipes"
      @toggle-favorite="toggleFavorite"
      @view-recipe="openRecipeModal"
    />
    <transition name="modal-fade">
      <RecipeModal v-if="selectedRecipe" :recipe="selectedRecipe" @close="closeRecipeModal" />
    </transition>
  </div>
</template>

<style scoped>
.page {
  max-width: 1260px;
  margin: 0 auto;
  padding: 2rem 1.25rem 3rem;
}

.hero {
  display: grid;
  grid-template-columns: 1.1fr 0.9fr;
  gap: 1.5rem;
  align-items: stretch;
  margin-bottom: 1.5rem;
  padding: 1.4rem;
  border-radius: 30px;
  background: linear-gradient(135deg, #fffaf3, #f3e7d8);
  border: 1px solid rgba(120, 85, 49, 0.08);
  box-shadow: 0 20px 50px rgba(79, 50, 26, 0.08);
}

.favorites-btn {
  border: 1px solid rgba(128, 93, 62, 0.12);
  background: #fffdf9;
  color: #6a5646;
  padding: 0.9rem 1.15rem;
  border-radius: 16px;
  font-size: 0.95rem;
  font-weight: 700;
  cursor: pointer;
  transition: all 0.2s ease;
  box-shadow: 0 10px 18px rgba(79, 50, 26, 0.05);
  white-space: nowrap;
}

.favorites-btn:hover {
  transform: translateY(-1px);
  border-color: rgba(181, 109, 56, 0.28);
  color: #2f2218;
}

.favorites-btn.active {
  background: linear-gradient(180deg, #d98c53, #c17139);
  color: white;
  border-color: transparent;
  box-shadow: 0 12px 22px rgba(193, 113, 57, 0.18);
}

.favorites-btn:focus {
  outline: none;
  box-shadow:
    0 0 0 4px rgba(193, 113, 57, 0.14),
    0 12px 22px rgba(193, 113, 57, 0.18);
}

.hero-content {
  display: flex;
  flex-direction: column;
  justify-content: center;
  padding: 1rem;
}

.hero-content h1 {
  margin: 0;
  font-family: Georgia, 'Times New Roman', serif;
  font-size: clamp(2.4rem, 5vw, 4.2rem);
  line-height: 0.95;
  letter-spacing: -0.04em;
  color: #2f2218;
  max-width: 9ch;
}

.hero-content p {
  margin: 1rem 0 0;
  max-width: 56ch;
  line-height: 1.8;
  color: #6c5645;
  font-size: 1rem;
}

.hero-image {
  width: 100%;
  height: 100%;
  min-height: 320px;
  border-radius: 24px;
  object-fit: cover;
  box-shadow: 0 16px 36px rgba(79, 50, 26, 0.12);
}

.controls {
  display: flex;
  flex-direction: column;
  gap: 1rem;
  margin: 1.5rem 0 1rem;
}

.top-row {
  display: grid;
  grid-template-columns: minmax(0, 1fr) auto;
  gap: 1rem;
  align-items: center;
}

.clear-btn {
  border: none;
  background: linear-gradient(180deg, #8a5933, #734927);
  color: white;
  padding: 0.9rem 1.2rem;
  border-radius: 16px;
  font-size: 0.95rem;
  font-weight: 700;
  cursor: pointer;
  transition: all 0.2s ease;
  box-shadow: 0 14px 28px rgba(115, 73, 39, 0.2);
  white-space: nowrap;
}

.clear-btn:hover {
  transform: translateY(-1px);
  background: linear-gradient(180deg, #99623a, #7d4f2b);
}

.clear-btn:active {
  transform: translateY(0);
}

.clear-btn:focus {
  outline: none;
  box-shadow:
    0 0 0 4px rgba(153, 98, 58, 0.18),
    0 14px 28px rgba(115, 73, 39, 0.2);
}

.controls {
  display: flex;
  flex-direction: column;
  gap: 1rem;
  margin: 1.5rem 0 1rem;
}

.top-row {
  display: grid;
  grid-template-columns: minmax(0, 1fr) auto auto;
  gap: 1rem;
  align-items: center;
}

.toggle-filters-btn {
  align-self: flex-start;
  border: 1px solid rgba(128, 93, 62, 0.12);
  background: #fffdf9;
  color: #6a5646;
  padding: 0.75rem 1rem;
  border-radius: 14px;
  font-size: 0.92rem;
  font-weight: 700;
  cursor: pointer;
  transition: all 0.2s ease;
  box-shadow: 0 10px 18px rgba(79, 50, 26, 0.05);
}

.toggle-filters-btn:hover {
  transform: translateY(-1px);
  border-color: rgba(153, 98, 58, 0.28);
  color: #2f2218;
}

.toggle-filters-btn:focus {
  outline: none;
  box-shadow:
    0 0 0 4px rgba(153, 98, 58, 0.12),
    0 10px 18px rgba(79, 50, 26, 0.05);
}

@media (max-width: 920px) {
  .top-row {
    grid-template-columns: 1fr;
  }

  .clear-btn,
  .favorites-btn {
    width: 100%;
  }
}

@media (max-width: 920px) {
  .hero {
    grid-template-columns: 1fr;
  }

  .clear-btn {
    width: 100%;
  }
}

.modal-fade-enter-active,
.modal-fade-leave-active {
  transition: all 0.25s ease;
}

.modal-fade-enter-from,
.modal-fade-leave-to {
  opacity: 0;
  transform: scale(0.96);
}

.filters-fade-enter-active,
.filters-fade-leave-active {
  transition: all 0.22s ease;
}

.filters-fade-enter-from,
.filters-fade-leave-to {
  opacity: 0;
  transform: translateY(-8px);
}

button {
  transition:
    transform 0.2s ease,
    box-shadow 0.2s ease,
    background 0.2s ease,
    border-color 0.2s ease,
    color 0.2s ease;
}
</style>
