<template>
  <div class="cart-wrapper">
    <div class="cart-items">
      <h2>Ваша корзина</h2>
      <div v-for="item in items" :key="item.id"class="cart-item">
        <img :src="item.image" alt="" class="cart-item-image" />
        <div class="cart-item-info">
          <h3>{{ item.name }}</h3>

          <div class="cart-item-meta">
            <span>{{ item.price }} ₽</span>

            <div class="quantity-controls">
            <button @click="$emit('decrease-quantity', item)">-</button>
            <span>{{ item.quantity }}</span>
            <button @click="$emit('increase-quantity', item)">+</button>
            </div>
          </div>

          <button class="remove-button" @click="$emit('remove-item', item)">Удалить</button>
        </div>
      </div>
    </div>






    <div class="cart-summary">
      <h3>Итого</h3>
      <p class="total">{{ totalPrice }} ₽</p>
      <button class="checkout-button" @click="checkout">Оформить заказ</button>
    </div>
  </div>
</template>

<script setup>
import { computed } from 'vue'


const props = defineProps({
  items: {                  
    type: Array,
    required: true
  }
})


const emit = defineEmits(['change-page'])       /** определение событий для компонента(то есть change page) */


const totalPrice = computed(() => {
  return props.items.reduce((acc, item) => acc + (item.price * item.quantity), 0)       /** вычисление суммы */
})


const checkout = () => {
  emit('change-page', 'checkout')  /** emit для смены страницы */
}
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;600&display=swap');

* {
  font-family: 'Inter', sans-serif;
}

.cart-wrapper {
  display: flex;
  flex-wrap: wrap;
  gap: 32px;
  padding: 32px;
  max-width: 1100px;
  margin: 48px auto;
  background-color: #ffffff;
  border: 2px solid #ccc;
  border-radius: 16px;
  box-shadow: 0 8px 24px rgba(0, 0, 0, 0.04);
  color: #1a202c;
}

.cart-items {
  flex: 3;
  min-width: 0;
  background-color: #ffffff;
  padding: 24px;
  border-radius: 12px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.06);
}

.cart-items h2 {
  margin-bottom: 20px;
  font-size: 1.5rem;
  font-weight: 600;
  color: #2d3748;
  text-align: center;
}

.cart-item {
  display: flex;
  align-items: center;
  border-bottom: 1px solid #e2e8f0;
  padding: 16px 0;
  gap: 20px;
}

.cart-item-image {
  flex-shrink: 0;
  width: 80px;
  height: 80px;
  object-fit: cover;
  border-radius: 8px;
}

.cart-item-info {
  flex: 1;
  min-width: 0;
  display: flex;
  flex-direction: column;
}

.cart-item-info h3 {
  font-size: 1rem;
  font-weight: 600;
  color: #2d3748;
  margin-bottom: 8px;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.cart-item-meta {
  display: flex;
  justify-content: space-between;
  align-items: center;
  color: #4a5568;
  font-weight: 500;
  margin-bottom: 8px;
}

.quantity-controls {
  display: flex;
  align-items: center;
  gap: 8px;
}

.quantity-controls button {
  padding: 4px 10px;
  background-color: #ffffff;
  border: 2px solid #2d3748;
  color: #00720f;
  font-weight: 700;
  border-radius: 6px;
  cursor: pointer;
  transition: all 0.2s ease;
}

.quantity-controls button:hover {
  background-color: #00720f;
  color: #ffffff;
}

.remove-button {
  align-self: flex-start;
  background-color: transparent;
  border: 2px solid #e53e3e;
  color: #e53e3e;
  font-weight: 600;
  padding: 4px 10px;
  border-radius: 6px;
  cursor: pointer;
  transition: all 0.2s ease;
}

.remove-button:hover {
  background-color: #e53e3e;
  color: #ffffff;
}

.cart-summary {
  flex: 1;
  background-color: #ffffff;
  padding: 24px;
  border-radius: 12px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.06);
  position: sticky;
  top: 20px;
  align-self: flex-start;
  min-width: 260px;
}

.cart-summary h3 {
  font-size: 1.25rem;
  font-weight: 600;
  margin-bottom: 16px;
  color: #2d3748;
}

.cart-summary .total {
  font-size: 1.75rem;
  font-weight: 700;
  margin-bottom: 24px;
  color: #1a202c;
}

.checkout-button {
  width: 100%;
  padding: 12px;
  background-color: #ffffff;
  color: #218c74;  /* Использую тот же цвет, что и для кнопок в хедере */
  border: 2px solid #218c74;  /* Цвет границы такой же */
  border-radius: 8px;
  font-weight: 700;
  cursor: pointer;
  transition: all 0.2s ease;
}

.checkout-button:hover {
  background-color: #218c74;
  color: white;
}

/* Адаптивность */
@media (max-width: 768px) {
  .cart-wrapper {
    flex-direction: column;
  }

  .cart-summary {
    position: static;
    margin-top: 32px;
  }
}
</style>
