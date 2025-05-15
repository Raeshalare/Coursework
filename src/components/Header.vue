<template>
  <div>
    <header class="header">
      <div class="logo">
        <img src="../assets/images/logo.png" alt="Логотип" class="logo-image" />
      </div>

      <nav class="nav">
        <div class="nav-left">
          <button @click="goHome" class="nav-button">Главная</button>
          <button @click="goCatalog" class="cat-button">Каталог</button>
        </div>

        <div class="nav-center">
          <div class="search">
            <input type="text" placeholder="Введите что-нибудь" class="search-input" v-model="modelSearch" @keyup.enter="performSearch" />
            <ul v-if="modelSearch.trim() && filteredSuggestions.length" class="suggestions">
              <li v-for="product in filteredSuggestions" :key="product.id" @click="selectSuggestion(product.name)">
                {{ product.name }}
              </li>
            </ul>
          </div>
        </div>

        <div class="nav-right">
          <button v-if="Authentication" @click="goToAccount" class="auth-button">Мой аккаунт</button>
          <button v-if="AdminOrNot" @click="goToAdminPage" class="auth-button">Админ-панель</button>
          <button v-if="!Authentication" class="auth-button" @click="$emit('open-login')">Войти</button>
          <button v-if="!Authentication" class="auth-button" @click="$emit('open-register')">Зарегистрироваться</button>
          <button v-if="Authentication" class="logout-button" @click="logout">Выйти</button>
          <button @click="goCart" class="cart-button">
            🛒<span class="cart-count">{{ cartItemsCount }}</span>
          </button>
        </div>
      </nav>
    </header>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

const props = defineProps({
  currentPage: String,
  products: Array,
  cartItems: Array,
  currentUser: Object
})

const emit = defineEmits(['change-page', 'search', 'open-register', 'open-login', 'logout'])

const modelSearch = ref('')

const Authentication = computed(() => !!props.currentUser)
const AdminOrNot = computed(() => props.currentUser && props.currentUser.role === 'admin')

const goHome = () => {
  modelSearch.value = ''  // Очищаем строку поиска при переходе на главную
  emit('change-page', 'home')
}

const goCatalog = () => {
  modelSearch.value = ''  // Очищаем строку поиска при переходе в каталог
  emit('change-page', 'catalog')
}

const goCart = () => emit('change-page', 'cart')
const goToAccount = () => emit('change-page', 'account')
const goToAdminPage = () => AdminOrNot.value && emit('change-page', 'admin')

const filteredSuggestions = computed(() =>
  modelSearch.value.trim()
    ? props.products.filter(p => p.name.toLowerCase().includes(modelSearch.value.toLowerCase())).slice(0, 5)
    : []
)

const cartItemsCount = computed(() =>
  props.cartItems.reduce((acc, item) => acc + item.quantity, 0)
)

const logout = () => emit('logout')

const performSearch = () => {
  emit('search', modelSearch.value)
  emit('change-page', 'catalog')
}

const selectSuggestion = (name) => {
  modelSearch.value = name
  emit('search', name)
}
</script>




<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;600&display=swap');

* {
  font-family: 'Inter', sans-serif;
}

.header {
  background: #f9fdfb;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.06);
  padding: 16px 24px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  border-bottom-left-radius: 16px;
  border-bottom-right-radius: 16px;
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  z-index: 1000;
}


@keyframes marquee {
  0% {
    transform: translateX(0);
  }
  100% {
    transform: translateX(-100%);
  }
}

body {
  margin-top: 140px; 
  background-color: #ffffff;
}

.logo-image {
  width: 72px;
  height: 72px;
  object-fit: contain;
}

.nav {
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 100%;
  flex-wrap: wrap;
  gap: 16px;
}

.nav-left, .nav-center, .nav-right {
  display: flex;
  align-items: center;
  gap: 12px;
}

.nav-left {
  margin-left: 32px;
}

.nav-right {
  margin-right: 32px;
}

.nav-button,
.cat-button,
.auth-button,
.logout-button,
.search-button {
  background-color: white;
  color: #218c74;
  padding: 8px 14px;
  border-radius: 8px;
  border: 1.5px solid #218c74;
  cursor: pointer;
  font-size: 0.9rem;
  font-weight: 600;
  transition: all 0.2s ease;
}

.nav-button:hover,
.cat-button:hover,
.auth-button:hover,
.logout-button:hover,
.search-button:hover {
  background-color: #218c74;
  color: white;
}

.logout-button:hover {
  background-color: #d63031;
  border-color: #d63031;
}

.search {
  flex: 1;
  display: flex;
  position: relative;
  max-width: 600px;
}

.search-input {
  flex: 1;
  padding: 10px 16px;
  border-radius: 8px;
  border: 1.5px solid #ccc;
  font-size: 0.9rem;
  color: #333;
  background-color: #ffffff;
  transition: border-color 0.2s ease;
}

.search-input:focus {
  border-color: #218c74;
  outline: none;
}

.search-button {
  margin-left: 8px;
}

.suggestions {
  position: absolute;
  top: 100%;
  left: 0;
  width: 100%;
  background-color: white;
  border: 1.5px solid #ccc;
  border-top: none;
  list-style: none;
  padding: 0;
  max-height: 200px;
  overflow-y: auto;
  z-index: 1001;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.06);
  border-radius: 0 0 8px 8px;
}

.suggestions li {
  padding: 10px 16px;
  cursor: pointer;
  transition: background-color 0.2s;
}

.suggestions li:hover {
  background-color: #f2fdf9;
}

.cart-button {
  position: relative;
  font-size: 1.6rem;
  color: #218c74;
  padding: 6px 12px;
  border-radius: 8px;
  border: 1.5px solid #218c74;
  background-color: white;
  cursor: pointer;
  transition: all 0.2s ease;
}

.cart-button:hover {
  background-color: #218c74;
  color: white;
}

.cart-count {
  position: absolute;
  top: -6px;
  right: -6px;
  background-color: #ff4757;
  color: white;
  font-size: 0.75rem;
  padding: 4px 6px;
  border-radius: 50%;
  font-weight: bold;
  line-height: 1;
}

@media (max-width: 768px) {
  .header {
    flex-direction: column;
    align-items: flex-start;
    padding: 12px;
  }

  .nav {
    flex-direction: column;
    align-items: stretch;
    width: 100%;
    gap: 12px;
  }

  .nav-left, .nav-right, .nav-center {
    margin: 0;
    justify-content: center;
    width: 100%;
  }

  .search {
    flex-direction: column;
  }

  .search-button {
    margin-left: 0;
    margin-top: 8px;
  }

  .cart-button {
    align-self: center;
  }
}
</style>

