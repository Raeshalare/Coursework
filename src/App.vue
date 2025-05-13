<template>
  <div>
    <Header v-model:search="searchQuery" :products="products" :cart-items="cartItems" :currentUser="currentUser" :current-page="currentPage" @change-page="handleChangePage" @search="handleSearch" @open-register="showRegister = true" @open-login="showLogin = true" @logout="handleLogout"/>



    <main>
      <!-- Показ страниц в зависимости от текущей активности -->
      <Catalog v-if="activePage === 'catalog'" :products="filteredProducts" @add-to-cart="addToCart" />
      <Cart v-if="activePage === 'cart'" :items="cartItems" @increase-quantity="increaseQuantity" @decrease-quantity="decreaseQuantity" @remove-item="removeItem" @change-page="handleChangePage" />
      <Home v-if="activePage === 'home'" />
      <UserPage v-if="activePage === 'account'" :user="currentUser" @update-user="updateUser" />
      <AdminPage v-if="activePage === 'admin'" :products="products" @update-products="updateProducts" />
      <!-- Страница оформления заказа -->
      <CheckoutPage v-if="activePage === 'checkout'" :cartItems="cartItems" :users="users" @confirm-order="handleConfirmOrder" />
    </main>
  </div>

  <!-- Модальные окна для регистрации и входа -->
  <RegisterModal :visible="showRegister" :users="users" @close="showRegister = false" @registered="addUser" />
  <LoginModal :visible="showLogin" :users="users" @close="closeLoginModal" @login-success="handleLoginSuccess" />
</template>

<script setup>
import { ref, computed } from 'vue'
import Header from './components/Header.vue'
import Catalog from './components/Catalog.vue'
import Cart from './components/CartPage.vue'
import Home from './components/MainPage.vue'
import RegisterModal from './components/RegisterModal.vue'
import LoginModal from './components/LoginModal.vue'
import UserPage from './components/UserPage.vue'
import AdminPage from './components/AdminPage.vue'
import CheckoutPage from './components/pageCheckout.vue'  


const showLogin = ref(false)
const currentUser = ref(null)

const handleLoginSuccess = (user) => {
  currentUser.value = user
  showLogin.value = false  
  if (user.role === 'admin') {
    activePage.value = 'admin'  
  } else {
    activePage.value = 'account'  
  }
}

const closeLoginModal = () => {
  showLogin.value = false  
}

const handleLogout = () => {
  currentUser.value = null
  activePage.value = 'home' 
}

const showRegister = ref(false)
const activePage = ref('home')
const searchQuery = ref('')

const users = ref([
  { login: 'admin', password: 'admin', firstName: 'Admin', lastName: 'admin', gender: 'admin', city: '', street: '', house: '', apartment: '', role: 'admin' },
])

const addUser = (user) => {
  users.value.push(user)
}

const updateUser = (updatedUser) => {
  const userIndex = users.value.findIndex(user => user.login === updatedUser.login)
  if (userIndex !== -1) {
    users.value[userIndex] = updatedUser
    currentUser.value = updatedUser  
  }
}

const products = ref([
  { id: 1, name: 'Парацетамол 500мг', price: 50, image: new URL('./assets/images/Catalog/paracetamol.webp', import.meta.url).href, category: 'Таблетки', description: 'Обезболивающее и жаропонижающее средство.' },
  { id: 2, name: 'Ибупрофен 200мг', price: 75, image: new URL('./assets/images/ibuprofen.webp', import.meta.url).href, category: 'Таблетки', description: 'Противовоспалительное и жаропонижающее средство.' },
  { id: 5, name: 'Амоксициллин 500мг', price: 120, image: new URL('./assets/images/Catalog/Amo.jpg ', import.meta.url).href, category: 'Антибиотики', description: 'Антибиотик для лечения бактериальных инфекций.' },
  { id: 6, name: 'Магний Б6', price: 150, image: new URL('./assets/images/Catalog/B6.jpg', import.meta.url).href, category: 'Витамины', description: 'Минеральный комплекс для нормализации работы мышц и нервной системы.' },
  { id: 7, name: 'Гепариновая мазь', price: 70, image: new URL('./assets/images/Catalog/Gepa.jpg', import.meta.url).href, category: 'Мази', description: 'Применяется при варикозном расширении вен и воспалении.' },
  { id: 8, name: 'Лоратадин 10мг', price: 60, image: new URL('./assets/images/Catalog/Lora.avif', import.meta.url).href, category: 'Антигистаминные препараты', description: 'Препарат для снятия симптомов аллергии.' },
  { id: 9, name: 'Тинидазол 500мг', price: 90, image: new URL('./assets/images/Catalog/Tini.jpg', import.meta.url).href, category: 'Антибиотики', description: 'Применяется для лечения инфекций мочевых путей и других бактериальных заболеваний.' },
  { id: 10, name: 'Аквамарис', price: 250, image: new URL('./assets/images/Catalog/Aqua.jpg', import.meta.url).href, category: 'Средства для носа', description: 'Солевой раствор для промывания носа и увлажнения слизистой.' },
  { id: 11, name: 'Омепразол 20мг', price: 120, image: new URL('./assets/images/Catalog/Omepra.png', import.meta.url).href, category: 'Препараты для желудка', description: 'Препарат для лечения заболеваний желудка и кишечника.' },
  { id: 12, name: 'Алоэ Вера гель', price: 80, image: new URL('./assets/images/Catalog/Aloe.jpg', import.meta.url).href, category: 'Косметика и уход', description: 'Гель для кожи на основе алоэ вера, применяется при ожогах и воспалениях.' },
  { id: 13, name: 'Рибоксин 200мг', price: 95, image: new URL('./assets/images/Catalog/Ribo.png', import.meta.url).href, category: 'Кардиопрепараты', description: 'Препарат для улучшения метаболизма сердечной мышцы.' },
  { id: 14, name: 'Левомицетин 250мг', price: 80, image: new URL('./assets/images/Catalog/Levo.jpg', import.meta.url).href, category: 'Антибиотики', description: 'Антибиотик для лечения бактериальных инфекций.' },
  { id: 15, name: 'Ромазулан', price: 120, image: new URL('./assets/images/Catalog/Roma.jpg', import.meta.url).href, category: 'Противовоспалительные препараты', description: 'Противовоспалительное средство для лечения воспалений слизистых оболочек.' },
  { id: 16, name: 'Глицерин 200мл', price: 45, image: new URL('./assets/images/Catalog/Gliz.webp', import.meta.url).href, category: 'Средства для кожи', description: 'Увлажняющее средство для ухода за кожей.' },
  { id: 17, name: 'Кеторол 10мг', price: 100, image: new URL('./assets/images/Catalog/Keto.jpg', import.meta.url).href, category: 'Обезболивающие препараты', description: 'Противовоспалительное обезболивающее средство.' },
  { id: 18, name: 'Валериана 50мг', price: 45, image: new URL('./assets/images/Catalog/Vale.jpg', import.meta.url).href, category: 'Препараты для нервной системы', description: 'Средство для нормализации нервной системы и снятия стресса.' },
  { id: 19, name: 'Бифидумбактерин', price: 200, image: new URL('./assets/images/Catalog/Bifi.webp', import.meta.url).href, category: 'Пробиотики', description: 'Препарат для восстановления микрофлоры кишечника.' },
  { id: 20, name: 'Цетиризин 10мг', price: 70, image: new URL('./assets/images/Catalog/Zetr.jpg', import.meta.url).href, category: 'Антигистаминные препараты', description: 'Антиаллергическое средство для борьбы с симптомами аллергии.' },
  { id: 21, name: 'Активированный уголь', price: 40, image: new URL('./assets/images/Catalog/Coal.webp', import.meta.url).href, category: 'Абсорбенты', description: 'Средство для очищения организма и устранения токсинов.' },
  { id: 22, name: 'Нимесулид 100мг', price: 130, image: new URL('./assets/images/Catalog/Nime.jpg', import.meta.url).href, category: 'Противовоспалительные препараты', description: 'Обезболивающее средство с противовоспалительным эффектом.' },
  { id: 23, name: 'Синекод сироп', price: 190, image: new URL('./assets/images/Catalog/Blue.webp', import.meta.url).href, category: 'Средства от кашля', description: 'Противокашлевое средство для лечения сухого кашля.' },
  { id: 24, name: 'Мелаксен 3мг', price: 280, image: new URL('./assets/images/Catalog/Meka.webp', import.meta.url).href, category: 'Препараты для сна', description: 'Средство для нормализации сна и улучшения сна.' },
  { id: 25, name: 'Бетадин мазь', price: 100, image: new URL('./assets/images/Catalog/Beta.jpg', import.meta.url).href, category: 'Противовоспалительные препараты', description: 'Мазь для лечения воспалений и инфекций кожи.' },
  { id: 26, name: 'Линекс', price: 320, image: new URL('./assets/images/Catalog/Line.jpg', import.meta.url).href, category: 'Пробиотики', description: 'Препарат для восстановления кишечной микрофлоры.' },
  { id: 27, name: 'Анаферон для детей', price: 180, image: new URL('./assets/images/Catalog/Anaf.jpg', import.meta.url).href, category: 'Противовирусные препараты', description: 'Препарат для профилактики и лечения вирусных заболеваний у детей.' },
  { id: 28, name: 'Фенистил гель', price: 220, image: new URL('./assets/images/Catalog/Feni.webp', import.meta.url).href, category: 'Средства от аллергии', description: 'Гель для снятия зуда и воспалений, вызванных аллергией.' },
  { id: 29, name: 'Мирамистин', price: 150, image: new URL('./assets/images/Catalog/Mira.jpg', import.meta.url).href, category: 'Антисептики', description: 'Антисептический раствор для лечения ран и ожогов.' },
  { id: 30, name: 'Витамин D3', price: 400, image: new URL('./assets/images/Catalog/Vita.jpg', import.meta.url).href, category: 'Витамины', description: 'Витамин для поддержания иммунной системы и здоровья костей.' },
  { id: 31, name: 'Пантенол спрей', price: 250, image: new URL('./assets/images/Catalog/Pant.webp', import.meta.url).href, category: 'Средства для кожи', description: 'Средство для восстановления кожи после ожогов и повреждений.' },
  { id: 32, name: 'Гевискон', price: 220, image: new URL('./assets/images/Catalog/Gevi.webp', import.meta.url).href, category: 'Препараты для желудка', description: 'Средство для лечения изжоги и гастроэзофагеального рефлюкса.' },
  { id: 33, name: 'Терафлю', price: 300, image: new URL('./assets/images/Catalog/Tera.jpg', import.meta.url).href, category: 'Простудные препараты', description: 'Средство для лечения простуды и гриппа, снимает симптомы.' },
  { id: 34, name: 'Мелоксикам 15мг', price: 110, image: new URL('./assets/images/Catalog/Melo.jpg', import.meta.url).href, category: 'Противовоспалительные препараты', description: 'Противовоспалительное средство для лечения суставных заболеваний.' },
  { id: 35, name: 'Энтеросгель', price: 250, image: new URL('./assets/images/Catalog/Ente.jpg', import.meta.url).href, category: 'Абсорбенты', description: 'Средство для очищения организма от токсинов и вредных веществ.' },
  { id: 36, name: 'Бандаж на колено с застёжкой', price: 950, image: new URL('./assets/images/Catalog/Band.jpg', import.meta.url).href, category: 'Ортопедическая продукция', description: 'Поддерживающий бандаж для коленного сустава, помогает при болях и травмах.' },
  { id: 37, name: 'Ортопедическая стелька Scholl GelActiv', price: 650, image: new URL('./assets/images/Catalog/Scho.jpg', import.meta.url).href, category: 'Ортопедическая продукция', description: 'Стельки для дополнительной амортизации и комфорта при ходьбе.' },
  { id: 38, name: 'Ортез для запястья', price: 1200, image: new URL('./assets/images/Catalog/Orte.webp', import.meta.url).href, category: 'Ортопедическая продукция', description: 'Ортез для фиксации запястья при растяжениях и вывихах.' },
  { id: 39, name: 'Но-шпа, 40мг №24', price: 120, image: new URL('./assets/images/noshpa.jpg', import.meta.url).href, category: 'Таблетки', description: 'Спазмолитическое средство для снятия боли и спазмов.' },
  { id: 40, name: 'Фервекс для взрослых', price: 190, image: new URL('./assets/images/fervex.jpg', import.meta.url).href, category: 'Простудные препараты', description: 'Порошок для лечения симптомов простуды и гриппа.' },
  { id: 41, name: 'Цитрамон П №10', price: 45, image: new URL('./assets/images/citramon.png', import.meta.url).href, category: 'Обезболивающие препараты', description: 'Комбинированный препарат для снятия головной боли и жара.'},
  { id: 42  , name: 'Смекта порошок', price: 130, image: new URL('./assets/images/smekta.webp', import.meta.url).href, category: 'Препараты для желудка', description: 'Сорбент для лечения диареи и расстройства пищеварения.' }
])

const filteredProducts = computed(() => {
  if (!searchQuery.value.trim()) return products.value
  return products.value.filter(p =>
    p.name.toLowerCase().includes(searchQuery.value.toLowerCase())
  )
})

const handleChangePage = (newPage) => {
  activePage.value = newPage
}

const handleSearch = (query) => {
  searchQuery.value = query
  activePage.value = 'catalog'
}

const cartItems = ref([])

const addToCart = (product) => {
  const existing = cartItems.value.find(item => item.id === product.id)
  if (existing) {
    existing.quantity += 1  
  } else {
    cartItems.value.push({ ...product, quantity: 1 })  
  }
}

const increaseQuantity = (product) => {
  const item = cartItems.value.find(p => p.id === product.id)
  if (item) item.quantity++
}

const decreaseQuantity = (product) => {
  const item = cartItems.value.find(p => p.id === product.id)
  if (item && item.quantity > 1) {
    item.quantity--
  }
}

const removeItem = (product) => {
  cartItems.value = cartItems.value.filter(p => p.id !== product.id)
}

const updateProducts = (updatedProducts) => {
  products.value = updatedProducts
}
</script>
