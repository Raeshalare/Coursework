<template>
  <div class="user-page">
    <h1>Добро пожаловать, {{ user.firstName }} {{ user.lastName }}!</h1>
    <div class="user-info">
      <form @submit.prevent="saveUserData">
        <div class="form-group">
          <label for="city">Город:</label>
          <input type="text" id="city" class="address-input" v-model="UserData.city" />
        </div>
        <div class="form-group">
          <label for="street">Улица:</label>
          <input type="text" id="street" class="address-input" v-model="UserData.street" />
        </div>
        <div class="form-group">
          <label for="house">Дом:</label>
          <input type="text" id="house" class="address-input" v-model="UserData.house" />
        </div>
        <div class="form-group">
          <label for="apartment">Квартира:</label>
          <input type="text" id="apartment" class="address-input" v-model="UserData.apartment" />
        </div>

        <button v-if="hasChanges || saved" type="submit" class="save-button" :class="{ saved: saved }" :disabled="saved">
          {{ saved ? 'Сохранено' : 'Сохранить' }}
        </button>
      </form>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, watch } from 'vue'

const props = defineProps({
  user: Object,
})

const emit = defineEmits(['update-user'])

const saved = ref(false)

const UserData = ref({
  city: props.user.city || '',
  street: props.user.street || '',
  house: props.user.house || '',
  apartment: props.user.apartment || ''
})

const hasChanges = computed(() => {
  return (
    UserData.value.city !== props.user.city ||
    UserData.value.street !== props.user.street ||
    UserData.value.house !== props.user.house ||    
    UserData.value.apartment !== props.user.apartment
  )
})

watch(UserData, () => {
  if (saved.value) {
    saved.value = false
  }
}, { deep: true })

const saveUserData = () => {
  emit('update-user', { ...UserData.value })
  saved.value = true
}
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;600&display=swap');

* {
  font-family: 'Inter', sans-serif;
}

.user-page {
  max-width: 600px;
  margin: 40px auto;
  padding: 24px;
  border: 1px solid #ccc;
  border-radius: 12px;
  background-color: #ffffff;
  color: #333;
}

.user-info {
  display: flex;
  flex-direction: column;
  gap: 12px;
}

.address-input {
  padding: 10px 16px;
  border-radius: 8px;
  border: 1.5px solid #ccc;
  font-size: 0.9rem;
  color: #333;
  background-color: #ffffff;
  transition: border-color 0.2s ease;
}

.address-input:focus {
  border-color: #218c74;
  outline: none;
}

.save-button {
  background-color: white;
  color: #218c74;
  padding: 10px 16px;
  border-radius: 8px;
  border: 1.5px solid #218c74;
  cursor: pointer;
  font-size: 0.9rem;
  margin: 16px 0;
  display: block;
  transition: all 0.3s ease;
  margin-left: 250px;
}

.save-button:hover {
  background-color: #218c74;
  color: white;
}

.save-button.saved {
  background-color: #00c96b;
  color: white;
  cursor: default;
  border-color: #008844;
}
</style>
