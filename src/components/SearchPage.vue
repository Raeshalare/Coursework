<template>
    <div class="search-page">
      <h2>Результаты поиска по запросу: "{{ searchQuery }}"</h2>
      <div class="search-results">
        <div v-for="product in filteredProducts" :key="product.id" class="product-card">
          <img :src="product.image" alt="product.name" class="product-image" />
          <h3>{{ product.name }}</h3>
          <p>{{ product.description }}</p>
          <p>Цена: {{ product.price }} ₽</p>
        </div>
      </div>
    </div>
  </template>
  
  <script setup>
  import { computed } from 'vue'
  
  const props = defineProps({
    searchQuery: {
      type: String,
      required: true
    }
  })
  
  const products = [
    { id: 1, name: 'Но-шпа', price: 150, image: 'noshpa.jpg', description: 'Обезболивающее средство, спазмолитик.' },
    { id: 2, name: 'Цитрамон', price: 90, image: 'citramon.jpg', description: 'Обезболивающее и жаропонижающее средство.' },
    { id: 3, name: 'Ибупрофен', price: 120, image: 'ibuprofen.jpg', description: 'Противовоспалительное средство.' },
  ]
  
  const filteredProducts = computed(() => {
  if (!props.searchQuery) {
    return products 
  }
  return products.filter(product =>
    product.name.toLowerCase().includes(props.searchQuery.toLowerCase()) ||
    product.description.toLowerCase().includes(props.searchQuery.toLowerCase())
  )
})

  </script>
  
  <style scoped>
  .search-page {
    padding: 20px;
  }
  
  .search-results {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 20px;
  }
  
  .product-card {
    border: 1px solid #ddd;
    padding: 16px;
    text-align: center;
  }
  
  .product-image {
    width: 100%;
    height: auto;
  }
  
  h2 {
    font-size: 1.5rem;
    margin-bottom: 20px;
  }
  </style>
  