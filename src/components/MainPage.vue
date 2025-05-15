<template>
  <div class="main-page">
    <div class="large-block">
      <img src="../assets/images/SuprastinBanner.jpg" alt="Большой блок" class="block-image" />
    </div>



    <div class="two-small-blocks">
      <div class="small-block">
        <img src="../assets/images/Linkas.jpg" alt="Малый блок 1" class="block-image" />
      </div>
      <div class="small-block">
        <img src="../assets/images/Oftalmoferon.jpg" alt="Малый блок 2" class="block-image" />
      </div>
    </div>



    <div class="tall-block">
      <img src="../assets/images/TromboMag.jpg" alt="Высокий блок" class="block-image" />
    </div>



    <div class="daily-products-carousel">
      <h2 class="section-title">Товары дня</h2>
      <div class="carousel-container">
        <button class="carousel-button left" @click="scrollLeft">&#10094;</button>

        <div class="carousel-wrapper" ref="carousel">
          <div class="carousel">
          <div class="product-card" v-for="(product, index) in dailyProducts" :key="index">
          <img :src="product.image" :alt="product.name" class="product-image" />
          <div class="product-info">
          <h3 class="product-name">{{ product.name }}</h3>
          <button class="buy-button">Купить</button>
          </div>
          </div>
          </div>
        </div>
        

        <button class="carousel-button right" @click="scrollRight">&#10095;</button>
      </div>
    </div>




<div class="reviews-carousel">
  <h2 class="section-title">Отзывы покупателей</h2>
  <div class="carousel-container">
    <button class="carousel-button left" @click="scrollReviewsLeft">&#10094;</button>

    <div class="carousel-wrapper" ref="reviewsCarousel">
      <div class="carousel">
      <div class="review-card" v-for="(review, index) in reviews" :key="index">
      <img :src="review.avatar" alt="Фото пользователя" class="review-avatar" />
      <p class="review-text">"{{ review.text }}"</p>
      <div class="review-stars">
      <span v-for="n in 5" :key="n" class="star" :class="{ filled: n <= review.rating }">★</span>
      </div>
      <p class="review-author">– {{ review.author }}</p>
      </div>
      </div>
    </div>
    <button class="carousel-button right" @click="scrollReviewsRight">&#10095;</button>
  </div>
</div>




  </div>
</template>

<script setup>
import { ref } from 'vue'
import { onMounted, onBeforeUnmount } from 'vue'

const carousel = ref(null)

const scrollLeft = () => {
  carousel.value?.scrollBy({
    left: -340,
    behavior: 'smooth',
  })
}

const scrollRight = () => {
  carousel.value?.scrollBy({
    left: 340,
    behavior: 'smooth',
  })
}

const dailyProducts = [
  { name: 'Но-шпа, 40мг №24', image: new URL('../assets/images/noshpa.jpg', import.meta.url).href, },
  { name: 'Фервекс для взрослых', image: new URL('../assets/images/fervex.jpg', import.meta.url).href, },
  { name: 'Цитрамон', image: new URL('../assets/images/citramon.png', import.meta.url).href, },
  { name: 'Ибупрофен 200мг', image: new URL('../assets/images/ibuprofen.webp', import.meta.url).href, },
  { name: 'Смекта порошок', image: new URL('../assets/images/smekta.webp', import.meta.url).href, },
]


const reviewsCarousel = ref(null)

const scrollReviewsLeft = () => {
  reviewsCarousel.value?.scrollBy({
    left: -320,
    behavior: 'smooth',
  })
}

const scrollReviewsRight = () => {
  reviewsCarousel.value?.scrollBy({
    left: 320,
    behavior: 'smooth',
  })
}

const reviews = [
  { text: 'Отличный сервис и быстрая доставка. Очень довольна!', author: 'Анна К.', rating: 5, avatar: new URL('../assets/images/anna.jpg', import.meta.url).href, },
  { text: 'Качественные товары по разумной цене!', author: 'Михаил Т.', rating: 4, avatar: new URL('../assets/images/mikhail.jpeg', import.meta.url).href, },
  { text: 'Всегда заказываю здесь, всё приходит вовремя.', author: 'Екатерина Л.', rating: 5, avatar: new URL('../assets/images/Kate.webp', import.meta.url).href, },
  { text: 'Очень доволен обслуживанием, спасибо!', author: 'Игорь Б.', rating: 4, avatar: new URL('../assets/images/Igor.webp', import.meta.url).href, },
  { text: 'Хороший выбор и понятный интерфейс.', author: 'Наталья С.', rating: 5, avatar: new URL('../assets/images/Natalie.jpg', import.meta.url).href, },
]

let autoScrollInterval = null

onMounted(() => {
  autoScrollInterval = setInterval(() => {
    reviewsCarousel.value?.scrollBy({ left: 320, behavior: 'smooth' })
  }, 3000)
})

onBeforeUnmount(() => {
  clearInterval(autoScrollInterval)
})
</script>


<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;600&display=swap');

* {
  font-family: 'Inter', sans-serif;
  box-sizing: border-box;
}

.main-page {
  margin-top: 140px;
  background-color: #ffffff;
}

.block-image {
  width: 100%;
  height: 100%;
  object-fit: cover;
  border-radius: 15px;
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.large-block, .small-block, .tall-block {
  border-radius: 15px;
  box-shadow: 0 6px 12px rgba(0, 0, 0, 0.08);
  overflow: hidden;
}

.large-block {
  height: 300px;
  margin-bottom: 20px;
}

.two-small-blocks {
  display: flex;
  justify-content: space-between;
  margin-bottom: 20px;
}

.small-block {
  width: 48%;
  height: 300px;
}

.tall-block {
  height: 400px;
  margin-bottom: 40px;
}

.daily-products-carousel {
  margin-top: 40px;
}

.section-title {
  font-size: 24px;
  font-weight: 600;
  margin-bottom: 20px;
  text-align: center;
  color: #333;
}

.carousel-container {
  position: relative;
  display: flex;
  align-items: center;
}

.carousel-wrapper {
  overflow: hidden;
  width: 100%;
}

.carousel {
  display: flex;
  gap: 20px;
  padding: 10px 0;
  transition: transform 0.3s ease;
}

.product-card {
  flex: 0 0 auto;
  width: 320px;
  height: 160px;
  background-color: #fff;
  border-radius: 15px;
  box-shadow: 0 6px 18px rgba(0, 0, 0, 0.08);
  display: flex;
  align-items: center;
  scroll-snap-align: start;
  transition: transform 0.3s ease;
}

.product-card:hover {
  transform: translateY(-4px);
}

.product-image {
  width: 40%;
  height: 100%;
  object-fit: contain;
  padding: 10px;
  background-color: #f9f9f9;
}

.product-info {
  width: 60%;
  padding: 10px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}

.product-name {
  font-size: 16px;
  font-weight: 500;
  color: #333;
  margin-bottom: 10px;
}

.buy-button {
  align-self: flex-start;
  background-color: #ffffff;
  color: #218c74;
  border: 1px solid #218c74;
  border-radius: 8px;
  padding: 6px 12px;
  font-size: 14px;
  cursor: pointer;
  transition: background-color 0.3s ease, color 0.3s ease;
}

.buy-button:hover {
  background-color: #218c74;
  color: white;
}

.carousel-button {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  background-color: rgba(30, 136, 229, 0.9);
  color: white;
  border: none;
  border-radius: 50%;
  width: 36px;
  height: 36px;
  font-size: 20px;
  cursor: pointer;
  z-index: 2;
  transition: background-color 0.3s ease;
}

.carousel-button {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  background-color: rgba(255, 255, 255, 0.7);
  color: #218c74;
  border: 1px solid #218c74;
  border-radius: 12px;
  width: 20px;
  height: 40px;
  font-size: 20px;
  font-weight: bold;
  cursor: pointer;
  z-index: 2;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.12);
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.3s ease;
}

.carousel-button:hover {
  background-color: #218c74;
  color: #fff;
  transform: translateY(-50%) scale(1.05);
  box-shadow: 0 6px 12px rgba(0, 0, 0, 0.15);
}

.carousel-button.left {
  left: -75px;
}

.carousel-button.right {
  right: -75px;
}


  .product-card {
    width: 280px;
  }

.reviews-carousel {
  margin-top: 60px;
}

.review-card {
  flex: 0 0 auto;
  width: 280px;
  background-color: #fff;
  border-radius: 15px;
  box-shadow: 0 6px 18px rgba(0, 0, 0, 0.08);
  padding: 20px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  scroll-snap-align: start;
  transition: transform 0.3s ease;
}

.review-card:hover {
  transform: translateY(-4px);
}

.review-text {
  font-size: 15px;
  color: #444;
  margin-bottom: 12px;
  line-height: 1.5;
}

.review-author {
  font-size: 14px;
  font-weight: 600;
  color: #218c74;
  text-align: right;
}

.review-avatar {
  width: 48px;
  height: 48px;
  border-radius: 50%;
  object-fit: cover;
  margin-bottom: 12px;
}

.review-stars {
  margin-bottom: 10px;
}

.star {
  font-size: 16px;
  color: #ccc;
  margin-right: 2px;
}

.star.filled {
  color: #FFD700; 
}

.block-image {
  width: 100%;
  height: 100%;
  object-fit: cover;
  border-radius: 15px;
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.large-block:hover .block-image,
.small-block:hover .block-image,
.tall-block:hover .block-image {
  transform: scale(1.05); 
  box-shadow: 0 12px 24px rgba(0, 0, 0, 0.12); 
}


</style>
