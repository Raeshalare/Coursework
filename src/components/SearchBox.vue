<template>
    <div class="search-box">
      <input
        v-model="searchQuery"
        @input="onInput"
        @focus="isDropdownVisible = true"
        @blur="hideDropdown"
        type="text"
        placeholder="Введите название"
        class="search-input"
      />
      <ul v-if="isDropdownVisible && filteredSuggestions.length" class="suggestions-list">
        <li
          v-for="suggestion in filteredSuggestions"
          :key="suggestion.id"
          @mousedown.prevent="selectSuggestion(suggestion)"
          class="suggestion-item"
        >
          {{ suggestion.name }}
        </li>
      </ul>
      <button :disabled="!searchQuery.trim()" @click="onSearch" class="search-button">
        Поиск
      </button>
    </div>
  </template>
  
  <script setup>
  import { ref, computed } from 'vue'
  
  const emit = defineEmits(['search'])
  
  const searchQuery = ref('')
  const isDropdownVisible = ref(false)
  

  const suggestions = ref([
    { id: 1, name: 'Но-шпа' },
    { id: 2, name: 'Цитрамон' },
    { id: 3, name: 'Ибупрофен' },   //подсказки для поиска
    { id: 4, name: 'Парацетамол' },
    { id: 5, name: 'Анальгин' },
  ])
  
  const filteredSuggestions = computed(() =>
    searchQuery.value
      ? suggestions.value.filter(s =>
          s.name.toLowerCase().includes(searchQuery.value.toLowerCase())
        )
      : []
  )
  
  const onSearch = () => {
    emit('search', searchQuery.value)
    isDropdownVisible.value = false
  }
  
  const onInput = () => {
    isDropdownVisible.value = true
  }
  
  const hideDropdown = () => {
    setTimeout(() => {
      isDropdownVisible.value = false
    }, 200)   
  }
  
  const selectSuggestion = (item) => {
    searchQuery.value = item.name
    onSearch()
  }
  </script>
  
  <style scoped>
  .search-box {
    position: relative;
    width: 300px;
    color: black;
  }
  
  .search-input {
    width: 100%;
    padding: 8px;
    border: 2px solid #009620;
    border-radius: 4px;
    font-size: 14px;
  }
  
  .suggestions-list {
    position: absolute;
    top: 100%;
    left: 0;
    right: 0;
    background: white;
    color: black;
    border: 1px solid #ccc;
    max-height: 150px;
    overflow-y: auto;
    z-index: 10;
    border-radius: 0 0 4px 4px;
  }
  
  .suggestion-item {
    padding: 8px;
    cursor: pointer;
    color: black;
  }
  
  .suggestion-item:hover {
    background-color: #f0f0f0;
  }
  
  .search-button {
    margin-top: 8px;
    padding: 6px 12px;
    background-color: #00720f;
    color: white;
    border: none;
    border-radius: 4px;
    cursor: pointer;
  }
  
  .search-button:disabled {
    background-color: #ccc;
    cursor: not-allowed;
  }
  </style>
  