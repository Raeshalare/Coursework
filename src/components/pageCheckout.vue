<template>
  <div class="checkout-wrapper">
    <div class="checkout-items">
      <h2>Ваш заказ</h2>
      <div
        v-for="item in cartItems"
        :key="item.id"
        class="checkout-item"
      >
        <img :src="item.image" alt="" class="checkout-item-image" />
        <div class="checkout-item-info">
          <h3>{{ item.name }}</h3>
          <p>{{ item.price }} ₽ x {{ item.quantity }}</p>
          <p class="checkout-item-total">{{ item.price * item.quantity }} ₽</p>
        </div>
      </div>


      <div v-if="showDuplicateModal" class="modal-overlay">
        <div class="modal">
        <h4>Адрес уже существует</h4>
        <p>Вы уже добавили этот адрес в список.</p>
        <button @click="showDuplicateModal = false" class="modal-close-button">Ок</button>
      </div>


</div>

    </div>

    <div class="address-selection">
      <h3>Выберите адрес доставки</h3>



      <div class="address-list">
        <div
          v-for="(address, index) in addresses"
          :key="index"
          class="address-card"
          :class="{ selected: selectedAddress === index }"
          @click="selectAddress(index)"
        >
          <p>{{ address.city }}, {{ address.street }}, {{ address.house }}, {{ address.apartment }}</p>
        </div>
      </div>



      <div class="new-address-form">
        <h4>Добавить новый адрес</h4>
        <label for="new-city">Город</label>
        <input v-model="newAddress.city" id="new-city" type="text" :class="{ 'input-error': !newAddress.city && formSubmitted }" />
        <label for="new-street">Улица</label>
        <input v-model="newAddress.street" id="new-street" type="text" :class="{ 'input-error': !newAddress.street && formSubmitted }" />
        <label for="new-house">Дом</label>
        <input v-model="newAddress.house" id="new-house" type="text" :class="{ 'input-error': !newAddress.house && formSubmitted }" />
        <label for="new-apartment">Квартира</label>
        <input v-model="newAddress.apartment" id="new-apartment" type="text" :class="{ 'input-error': !newAddress.apartment && formSubmitted}" />

        <button @click="addNewAddress" class="add-address-button">Добавить адрес</button>
      </div>

      <button :disabled="selectedAddress === null" class="place-order-button">Подтвердить заказ</button>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'


const showDuplicateModal = ref(false)
const props = defineProps({
  cartItems: Array
})

const addresses = ref([

])

const selectedAddress = ref(null)
const newAddress = ref({
  city: '',
  street: '',
  house: '',
  apartment: ''
})

const formSubmitted = ref(false)  


const selectAddress = (index) => {
  selectedAddress.value = index
}


const addNewAddress = () => {
  formSubmitted.value = true

  const { city, street, house, apartment } = newAddress.value
  if (!city || !street || !house || !apartment) {
    return
  }

  const isDuplicate = addresses.value.some(addr =>
    addr.city === city &&
    addr.street === street &&
    addr.house === house &&
    addr.apartment === apartment
  )

  if (isDuplicate) {
    showDuplicateModal.value = true
    return
  }

  addresses.value.push({ ...newAddress.value })
  newAddress.value = { city: '', street: '', house: '', apartment: '' }
  formSubmitted.value = false
}

</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;600&display=swap');

* {
  font-family: 'Inter', sans-serif;
}

.checkout-wrapper {
  display: flex;
  justify-content: space-between;
  gap: 2rem;
  padding: 2rem;
  background-color: #f9fdfb;
  border-radius: 8px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.06);
  color: #00720f;
}

.checkout-items {
  width: 60%;
  background-color: white;
  padding: 1.5rem;
  border-radius: 8px;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
}

.checkout-item {
  display: flex;
  justify-content: space-between;
  padding: 1rem 0;
  border-bottom: 1px solid #f0f0f0;
}

.checkout-item:last-child {
  border-bottom: none;
}

.checkout-item-image {
  width: 60px;
  height: 60px;
  object-fit: cover;
  border-radius: 4px;
}

.checkout-item-info {
  flex-grow: 1;
  margin-left: 1rem;
}

.checkout-item-info h3 {
  font-size: 1rem;
  font-weight: bold;
}

.checkout-item-info p {
  margin: 4px 0;
  font-size: 0.9rem;
}

.checkout-item-total {
  font-size: 1.1rem;
  color: #00720f;
  font-weight: bold;
}

.address-selection {
  width: 35%;
  background-color: white;
  padding: 1.5rem;
  border-radius: 8px;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
}

.address-selection h3 {
  font-size: 1.3rem;
  font-weight: bold;
  margin-bottom: 1rem;
}

.address-list {
  margin-bottom: 1rem;
}

.address-card {
  padding: 1rem;
  border: 1.5px solid #218c74;
  border-radius: 6px;
  margin-bottom: 1rem;
  cursor: pointer;
  transition: background-color 0.3s;
}

.address-card:hover {
  background-color: #f5f5f5;
}

.address-card.selected {
  background-color: #c8e6c9;
  border-color: #81c784;
}

.new-address-form {
  margin-top: 1.5rem;
  margin-bottom: 1.5rem;
}

.new-address-form h4 {
  font-size: 1.1rem;
  font-weight: bold;
  margin-bottom: 1rem;
}

.new-address-form label {
  font-size: 0.9rem;
  font-weight: 500;
  margin-bottom: 0.5rem;
  display: block;
}

.new-address-form input {
  width: 90%;
  padding: 0.8rem;
  margin-bottom: 1rem;
  border: 1.5px solid #ccc;
  border-radius: 6px;
  font-size: 0.9rem;
  background-color: white;
  color: #00720f;
}

.new-address-form input:focus {
  border-color: #218c74;
  outline: none;
}

.input-error {
  border-color: #f83223;
  background-color: #ffebee;
}

.add-address-button {
  background-color: #218c74;
  color: white;
  padding: 0.8rem;
  width: 100%;
  border: 1.5px solid #218c74;
  border-radius: 6px;
  cursor: pointer;
  font-weight: bold;
  transition: background-color 0.3s;
}

.add-address-button:hover {
  background-color: white;
  color: #218c74;
  border-color: #218c74;
}

.place-order-button {
  background-color: white;
  color: #218c74;
  padding: 1rem;
  width: 100%;
  border: 1.5px solid #218c74;
  border-radius: 6px;
  cursor: pointer;
  font-weight: bold;
  transition: background-color 0.3s;
}

.place-order-button:disabled {
  background-color: #b0bec5;
  cursor: not-allowed;
}

.place-order-button:hover:enabled {
  background-color: #218c74;
  color: white;
}

.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: #e0e0e09d;
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 999;
}

.modal {
  background-color: white;
  padding: 2rem;
  border-radius: 10px;
  width: 320px;
  text-align: center;
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.2);
  border: 3px solid #218c74;
}

.modal h4 {
  margin-bottom: 1.2rem;
  color: #218c74;
  font-size: 1.1rem;
  font-weight: bold;
}

.modal-close-button {
  margin-top: 1rem;
  background-color: #4caf50;
  color: white;
  border: none;
  padding: 0.8rem 1.4rem;
  border-radius: 6px;
  cursor: pointer;
  font-weight: bold;
  transition: background-color 0.3s;
}

.modal-close-button:hover {
  background-color: #388e3c;
}

</style>



