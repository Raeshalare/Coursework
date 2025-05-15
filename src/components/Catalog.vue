<template>
  <div class="catalog-container">
    <aside class="filters">
      <h2>Фильтры</h2>

      <div class="filter-group">
        <label>Категории:</label>
        <div v-for="category in categories" :key="category">
          <input type="checkbox" :value="category" v-model="selectedCategories" />{{ category }}</div>
      </div>

      <div class="filter-price">
        <label>Макс. цена: {{ maxPrice }} ₽</label>
        <input type="range" min="0" :max="priceLimit" v-model="maxPrice" />
      </div>
    </aside>

    <div class="product-grid">
      <div v-for="product in paginatedProducts" :key="product.id" class="product-card">
        <img :src="product.image" alt="" class="product-image" />
        <h3 class="product-name">{{ product.name }}</h3>
        <p class="product-price">{{ product.price }} ₽</p>
        <button class="btn product-button" @click="openModal(product)">Подробнее</button>
      </div>
    </div>


    <div class="pagination">
      <button 
        @click="changePage(currentPage - 1)" 
        :disabled="currentPage === 1"
        class="pagination-button">Назад</button>
      

      <div class="page-numbers">
        <button 
          v-for="page in totalPages" 
          :key="page" 
          @click="changePage(page)"
          :class="{'active': currentPage === page}" 
          class="pagination-button">
          {{ page }}
        </button>
      </div>

      <button 
        @click="changePage(currentPage + 1)" 
        :disabled="currentPage === totalPages"
        class="pagination-button">Вперёд</button>
    </div>
  </div>


  <div v-if="selectedProduct" class="modal-overlay" @click.self="closeModal">
    <div class="modal-content">
      <span class="close-button" @click="closeModal">×</span>
      <h2>{{ selectedProduct.name }}</h2>
      <img :src="selectedProduct.image" alt="" class="modal-image" />
      <p>{{ selectedProduct.description }}</p>
      <p class="product-price">{{ selectedProduct.price }} ₽</p>
      <button class="btn add-button" @click="emit('add-to-cart', selectedProduct)">Добавить в корзину</button>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

const emit = defineEmits(['add-to-cart']);
const props = defineProps({
  products: Array
})

const selectedProduct = ref(null)
const selectedCategories = ref([])
const maxPrice = ref(1000)
const currentPage = ref(1)
const itemsPerPage = 30

const categories = [...new Set(props.products.map(p => p.category || 'Без категории'))]

const priceLimit = Math.max(...props.products.map(p => p.price || 0))

const filteredProducts = computed(() => {
  return props.products.filter(product => {
    const inCategory = selectedCategories.value.length === 0 || selectedCategories.value.includes(product.category)
    const underPrice = product.price <= maxPrice.value
    return inCategory && underPrice
  })
})

const totalPages = computed(() => {
  return Math.ceil(filteredProducts.value.length / itemsPerPage)
})

const paginatedProducts = computed(() => {
  const startIndex = (currentPage.value - 1) * itemsPerPage
  const endIndex = startIndex + itemsPerPage
  return filteredProducts.value.slice(startIndex, endIndex)
})

function openModal(product) {
  selectedProduct.value = product
}

function closeModal() {
  selectedProduct.value = null
}

function changePage(page) {
  if (page < 1 || page > totalPages.value) return
  currentPage.value = page
}
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;600&display=swap');

* {
  font-family: 'Inter', sans-serif;
}

.catalog-container {
  display: flex;
  gap: 24px;
  padding: 80px 20px 20px;
  box-sizing: border-box;
  flex-wrap: wrap;
}

.filters {
  width: 250px;
  background-color: #f9fdfb;
  color: #218c74;
  padding: 24px;
  border-radius: 12px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.06);
  overflow-y: auto;
  max-height: calc(100vh - 120px);
}

.filter-group,
.filter-price {
  margin-bottom: 24px;
}

.filter-group label,
.filter-price label {
  font-weight: 600;
  color: #2d3748;
  
}

.product-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 20px;
  flex: 1;
  margin-top: 0;
}

@media (max-width: 1200px) {
  .product-grid {
    grid-template-columns: repeat(2, 1fr);
  }
}

@media (max-width: 768px) {
  .catalog-container {
    flex-direction: column;
  }

  .product-grid {
    grid-template-columns: repeat(2, 1fr);
  }
}

.product-card {
  background-color: #ffffff;
  border-radius: 16px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.06);
  padding: 16px;
  display: flex;
  flex-direction: column;
  transition: transform 0.2s ease, box-shadow 0.2s ease;
}

.product-card:hover {
  transform: translateY(-4px);
  box-shadow: 0 8px 16px rgba(0, 0, 0, 0.15);
}

.product-image {
  width: 100%;
  height: 160px;
  object-fit: cover;
  border-radius: 12px;
  margin-bottom: 12px;
}

.product-name {
  font-size: 18px;
  font-weight: 600;
  margin-bottom: 8px;
  color: #2d3748;
}

.product-price {
  color: #218c74;
  font-size: 16px;
  font-weight: bold;
  margin-bottom: 12px;
}

.btn {
  padding: 6px 12px;
  border-radius: 8px;
  border: 1.5px solid #218c74;
  background-color: white;
  color: #218c74;
  cursor: pointer;
  transition: all 0.3s ease;
  margin-top: auto;
}

.btn:hover {
  background-color: #218c74;
  color: white;
}

.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.4);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 1000;
}

.modal-content {
  background: white;
  color: black;
  padding: 24px;
  border-radius: 12px;
  max-width: 400px;
  width: 90%;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.3);
  position: relative;
}

.modal-image {
  width: 100%;
  height: 200px;
  object-fit: cover;
  border-radius: 8px;
  margin-bottom: 12px;
}

.close-button {
  position: absolute;
  top: 12px;
  right: 16px;
  font-size: 24px;
  font-weight: bold;
  color: #d63031;
  cursor: pointer;
  background: transparent;
  border: none;
  transition: transform 0.2s ease;
}

.close-button:hover {
  transform: scale(1.2);
}

.pagination {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 10px;
  margin-top: 20px;
  width: 100%;
}

.pagination-button {
  padding: 8px 16px;
  border: 1.5px solid #218c74;
  background-color: white;
  color: #218c74;
  cursor: pointer;
  border-radius: 8px;
  transition: background-color 0.3s ease, color 0.3s ease;
}

.pagination-button:hover {
  background-color: #218c74;
  color: white;
}

.pagination-button:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}

.page-numbers {
  display: flex;
  gap: 8px;
}

.page-numbers .pagination-button.active {
  background-color: #218c74;
  color: white;
}

.catalog-container {
  display: flex;
  gap: 2rem;
}

.filters {
  width: 250px;
  padding: 1rem;
  background-color: #f9f9f9;
  border-radius: 12px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}

.filters h2 {
  margin-bottom: 1rem;
  font-size: 1.2rem;
  font-weight: bold;
  color: #333;
}

.filter-group,
.filter-price {
  margin-bottom: 1.5rem;
}

.filter-group label,
.filter-price label {
  font-weight: 500;
  display: block;
  margin-bottom: 0.5rem;
}

.filter-group div {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  margin-bottom: 0.3rem;
}

input[type="checkbox"] {
accent-color: #218c74;
  width: 16px;
  height: 16px;
}

input[type="range"] {
  width: 100%;
  accent-color: #218c74;
}

</style>
