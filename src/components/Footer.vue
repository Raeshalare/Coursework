<template>
  <div class="footer">
    <div class="column">
      <h3>О нас</h3>
      <p>Мы предлагаем широкий выбор медицинских товаров, включая лекарства, оборудование и ортопедическую продукцию.</p>
    </div>
      <div class="column">
      <h3>Полезные ссылки</h3>
      <ul>
      <li><a href="#">Главная</a></li>
      <br>
      <li><a href="#">Каталог</a></li>
      <br>
      <li><a href="#">Контакты</a></li>
      <br>
      <li><a href="#">О нас</a></li>
      </ul>
      </div>
    <div class="column">
      <h3>Контакты</h3>
      <p>Телефон: +7 (123) 456-78-90</p>
      <p>Email: info@vuemed.com</p>
    </div>
    <div class="column">
      <h3>Следите за нами</h3>
      <p>Мы в социальных сетях:</p>
      <ul>
      <li><a href="#" class="social-link"><img src="https://www.adabible.org/wp-content/uploads/2016/11/FB-logo.png" class="social-icon" />Facebook</a></li>
      <li><a href="#" class="social-link"><img src="https://www.detaytemizlik.com.tr/view/images/instagram.png" class="social-icon" />Instagram</a></li>
      <li><a href="#" class="social-link"><img src="https://media.d3.nhle.com/image/private/t_w_720/f_png/prd/q68azdipmpubyd0cio4w.png" alt="Twitter" class="social-icon" />Twitter</a></li>
      </ul>
  </div>
  </div>
</template>

<script setup>
</script>

<style scoped>
.footer {
  width: 100vw;
  position: relative;
  left: 50%;
  right: 50%;
  margin-left: -50vw;
  margin-right: -50vw;
  background-color: #218c74; 
  color: white; 
  padding: 16px 24px;
  font-size: 14px;
  font-weight: 500;
  font-family: 'Inter', sans-serif;
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
  align-items: flex-start;
  box-sizing: border-box;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
  border-top-left-radius: 16px;
  border-top-right-radius: 16px;
  
  z-index: 100;
}


.footer .column {
  flex: 1;
  min-width: 200px;
  padding: 10px 20px;
}

.footer h3 {
  margin-bottom: 14px;
  font-weight: 600;
  font-size: 1.1rem;
  border-bottom: 2px solid rgba(255, 255, 255, 0.3);
  padding-bottom: 6px;
}

.footer p,
.footer ul {
  margin: 0;
  padding: 0;
  list-style: none;
}

.footer ul li {
  margin: 8px 0;
}

.footer a {
  color: white;
  text-decoration: none;
  font-weight: 500;
  transition: color 0.3s ease;
}

.footer a:hover {
  text-decoration: underline;
  color: #d1f7e0;
}

.social-link {
  display: flex;
  align-items: center;
  gap: 8px;
  color: white;
  font-weight: 500;
  text-decoration: none;
  transition: color 0.3s ease;
}

.social-link:hover {
  color: #d1f7e0;
}

.social-icon {
  width: 24px;
  height: 24px;
  object-fit: contain;
  filter: drop-shadow(0 0 1px rgba(0, 0, 0, 0.2));
  transition: filter 0.3s ease;
}

.social-link:hover .social-icon {
  filter: drop-shadow(0 0 4px #a0f0b8);
}



</style>
