<template>
  <div v-if="visible" class="modal-overlay" @click.self="close">
    <div class="modal-content">
      <h2>Регистрация</h2>
      <form @submit.prevent="register">
        <div class="form-group">
          <label>Логин:</label>
          <input v-model="form.login" type="text" class="input-field" />
          <span v-if="errors.login" class="error">{{ errors.login }}</span>
        </div>

        <div class="form-group">
          <label>Пароль:</label>
          <input v-model="form.password" type="password" class="input-field" />
          <span v-if="errors.password" class="error">{{ errors.password }}</span>
        </div>

        <div class="form-group">
          <label>Имя:</label>
          <input v-model="form.firstName" type="text" class="input-field" />
          <span v-if="errors.firstName" class="error">{{ errors.firstName }}</span>
        </div>

        <div class="form-group">
          <label>Фамилия:</label>
          <input v-model="form.lastName" type="text" class="input-field" />
          <span v-if="errors.lastName" class="error">{{ errors.lastName }}</span>
        </div>

        <div class="form-group">
          <label>Пол:</label>
          <select v-model="form.gender" class="input-field">
            <option value="">Выберите пол</option>
            <option value="Мужской">Мужской</option>
            <option value="Женский">Женский</option>
          </select>
          <span v-if="errors.gender" class="error">{{ errors.gender }}</span>
        </div>

        <div class="form-actions">
          <button type="submit" class="btn register-btn">Зарегистрироваться</button>
          <button type="button" class="btn register-btn" @click="close">Отмена</button>
        </div>
      </form>
    </div>
  </div>
</template>

<script setup>
import { reactive } from 'vue'

const props = defineProps({
  visible: Boolean,
  users: Array
})

const emit = defineEmits(['close', 'registered'])

const form = reactive({
  login: '',
  password: '',
  firstName: '',
  lastName: '',
  gender: ''
})

const errors = reactive({
  login: '',
  password: '',
  firstName: '',
  lastName: '',
  gender: ''
})

function close() {
  emit('close')
}

function validate() {
  let valid = true
  errors.login = form.login ? '' : 'Обязательное поле'
  errors.password = /^[A-Za-z0-9]+$/.test(form.password) ? '' : 'Только латинские буквы и цифры'
  errors.firstName = form.firstName ? '' : 'Обязательное поле'
  errors.lastName = form.lastName ? '' : 'Обязательное поле'
  errors.gender = form.gender ? '' : 'Выберите пол'


  if (form.login.toLowerCase() === 'admin') {
    errors.login = 'Логин "admin" запрещен'
    valid = false
  }

  if (form.password.toLowerCase() === 'admin') {
    errors.password = 'Пароль "admin" запрещен'
    valid = false
  }

  for (let key in errors) {
    if (errors[key]) valid = false
  }

  return valid
}

function register() {
  if (!validate()) return

  emit('registered', { ...form }) 
  Object.assign(form, {login: '', password: '', firstName: '', lastName: '', gender: ''  }) // Очищаем форму
  close()
}
</script>


<style scoped>
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  background: rgba(0, 0, 0, 0.4);
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
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
  transition: all 0.3s ease;
}

h2 {
  text-align: center;
  margin-bottom: 20px;
  font-weight: 600;
  font-size: 1.2rem;
  color: #218c74;
}

.form-group {
  margin-bottom: 12px;
}

.input-field {
  width: 90%;
  padding: 10px 16px;
  border-radius: 8px;
  border: 1.5px solid #ccc;
  font-size: 1rem;
  color: #333;
  background-color: #ffffff;
  transition: border-color 0.2s ease;
}

.input-field:focus {
  border-color: #218c74;
  outline: none;
}

.error {
  color: red;
  font-size: 0.9em;
}

.form-actions {
  display: flex;
  justify-content: center;
  gap: 16px;
  margin-top: 16px;
}

button {
  padding: 10px 16px;
  background-color: white;
  color: #218c74;
  border-radius: 8px;
  border: 1.5px solid #218c74;
  cursor: pointer;
  font-size: 1rem;
  font-weight: 600;
  transition: all 0.2s ease;
}

button:hover {
  background-color: #218c74;
  color: white;
}

button[type="button"] {
  background-color: #f2f2f2;
  color: #555;
}

button[type="button"]:hover {
  background-color: #ddd;
  color: #333;
}

@media (max-width: 768px) {
  .modal-content {
    padding: 16px;
    max-width: 100%;
  }

  h2 {
    font-size: 1rem;
  }

  .input-field {
    padding: 8px 12px;
  }

  button {
    font-size: 0.9rem;
  }
}
</style>
