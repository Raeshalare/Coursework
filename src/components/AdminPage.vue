<template>
  <div class="admin-page">
    <h2>Админ-панель</h2>
    <form @submit.prevent="addProduct" class="product-form">
      <div class="form-group">
        <label for="product-name">Название товара:</label>
        <input type="text" id="product-name" v-model="newProduct.name" class="input-field" />
        <span v-if="errors.name" class="error-message">{{ errors.name }}</span>
      </div>
      <div class="form-group">
        <label for="product-price">Цена:</label>
        <input type="number" id="product-price" v-model="newProduct.price" class="input-field" />
        <span v-if="errors.price" class="error-message">{{ errors.price }}</span>
      </div>
      <div class="form-group">
        <label for="product-image">Ссылка на изображение:</label>
        <input type="text" id="product-image" v-model="newProduct.image" class="input-field" />
        <span v-if="errors.image" class="error-message">{{ errors.image }}</span>
      </div>
      <div class="form-group">
        <label for="product-category">Категория:</label>
        <input type="text" id="product-category" v-model="newProduct.category" class="input-field" />
        <span v-if="errors.category" class="error-message">{{ errors.category }}</span>
      </div>
      <div class="form-group">
        <label for="product-description">Описание:</label>
        <input type="text" id="product-description" v-model="newProduct.description" class="textarea-field"/>
        <span v-if="errors.description" class="error-message">{{ errors.description }}</span>
      </div>
      <button type="submit" class="submit-button">Добавить товар</button>
    </form>
  </div>
</template>

<script setup>
import { ref, defineProps, defineEmits } from 'vue'

const props = defineProps({
  products: Array
})

const emit = defineEmits(['update-products'])

const newProduct = ref({
  name: '',
  price: '',
  image: '',
  category: '',
  description: ''
})

const errors = ref({
  name: '',
  price: '',
  image: '',
  category: '',
  description: ''
})

const validateForm = () => {
  let isValid = true
  // Очистим ошибки
  errors.value = {
    name: '',
    price: '',
    image: '',
    category: '',
    description: ''
  }

  // Проверка на пустые поля
  if (!newProduct.value.name) {
    errors.value.name = 'Введите название товара.'
    isValid = false
  }
  if (!newProduct.value.price) {
    errors.value.price = 'Введите цену товара.'
    isValid = false
  }
  if (!newProduct.value.category) {
    errors.value.category = 'Введите категорию товара.'
    isValid = false
  }
  if (!newProduct.value.description) {
    errors.value.description = 'Введите описание товара.'
    isValid = false
  }

  return isValid
}

const addProduct = () => {
  if (validateForm()) {
    const id = props.products.length + 1
    const product = {
      id,
      name: newProduct.value.name,
      price: newProduct.value.price,
      image: newProduct.value.image,
      category: newProduct.value.category,
      description: newProduct.value.description
    }
    props.products.push(product)
    emit('update-products', props.products)
    newProduct.value = { name: '', price: '', image: '', category: '', description: '' }
  }
}
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;600&display=swap');

* {
  font-family: 'Inter', sans-serif;
}

.admin-page {
  max-width: 1000px;
  margin: 48px auto;
  padding: 32px;
  border: 1.5px solid #e0e0e0;
  border-radius: 16px;
  background-color: #ffffff;
  box-shadow: 0 8px 24px rgba(0, 0, 0, 0.04);
  color: #333;
}

h2 {
  font-size: 1.5rem;
  font-weight: 600;
  color: #218c74;
  margin-bottom: 24px;
  text-align: center;
}

.product-form {
  display: flex;
  flex-direction: column;
  gap: 20px;
}

.form-group {
  display: flex;
  flex-direction: column;
}

label {
  font-size: 0.95rem;
  font-weight: 500;
  margin-bottom: 6px;
  color: #444;
}

.input-field,
.textarea-field {
  width: 90%;
  padding: 10px 14px;
  border-radius: 8px;
  border: 1.5px solid #ccc;
  font-size: 0.95rem;
  color: #333;
  background-color: #fefefe;
  transition: border-color 0.2s ease;
}

.input-field:focus,
.textarea-field:focus {
  border-color: #218c74;
  outline: none;
}

.textarea-field {
  height: 120px;
  resize: none;

}

.error-message {
  color: #e74c3c;
  font-size: 0.85rem;
  margin-top: 4px;
}

.submit-button {
  align-self: flex-start;
  background-color: #218c74;
  color: white;
  padding: 12px 24px;
  border-radius: 8px;
  border: none;
  font-size: 1rem;
  font-weight: 600;
  cursor: pointer;
  transition: background-color 0.2s ease;
}

.submit-button:hover {
  background-color: #176b5e;
}

</style>
