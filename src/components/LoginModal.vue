<template>
  <div v-if="visible" class="modal-overlay">
    <div class="modal-content">
      <h2 class="modal-title">Вход</h2>
      <form @submit.prevent="login">
        <div class="form-group">
          <input
            v-model="loginData.login"
            type="text"
            placeholder="Логин"
            class="input-field"
          />
        </div>
        <div class="form-group">
          <input
            v-model="loginData.password"
            type="password"
            placeholder="Пароль"
            class="input-field"
          />
        </div>
        <div class="error" v-if="error">{{ error }}</div>
        <div class="form-actions">
          <button type="submit" class="auth-button">Войти</button>
          <button type="button" @click="$emit('close')" class="auth-button">Отмена</button>
        </div>
      </form>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const props = defineProps({
  visible: Boolean,
  users: Array,
})
const emit = defineEmits(['close', 'login-success'])

const loginData = ref({
  login: '',
  password: '',
})

const error = ref('')

const login = () => {
  const user = props.users.find(
    (u) => u.login === loginData.value.login && u.password === loginData.value.password
  )
  if (user) {
    emit('login-success', user)
    loginData.value = { login: '', password: '' }
    error.value = ''
  } else {
    error.value = 'Неверный логин или пароль'
  }
}
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;600&display=swap');

* {
  font-family: 'Inter', sans-serif;
}

.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  background: rgba(0, 0, 0, 0.4);
  color: #00720f;
  width: 100%;
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 999;
}

.modal-content {
  background: white;
  padding: 24px;
  border-radius: 12px;
  width: 100%;
  max-width: 400px;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.3);
}

.modal-title {
  font-size: 24px;
  font-weight: bold;
  margin-bottom: 16px;
  text-align: center;
}

.form-group {
  margin-bottom: 12px;
}

.input-field {
  width: 90%;
  padding: 10px 16px;
  border-radius: 8px;
  border: 1.5px solid #ccc;
  font-size: 0.9rem;
  background-color: #ffffff;
  color: #333;
  transition: border-color 0.2s ease;
}

.input-field:focus {
  border-color: #218c74;
  outline: none;
}

.error {
  color: red;
  font-size: 0.9em;
  text-align: center;
}

.form-actions {
  display: flex;
  justify-content: center;
  margin-top: 16px;
}

.auth-button {
  padding: 10px 16px;
  background-color: #ffffff;
  color: #218c74;
  border: 1.5px solid #218c74;
  border-radius: 8px;
  cursor: pointer;
  font-size: 0.9rem;
  font-weight: 600;
  transition: all 0.2s ease;
  margin: 0 8px;
}

.auth-button:hover {
  background-color: #218c74;
  color: white;
}

</style>
