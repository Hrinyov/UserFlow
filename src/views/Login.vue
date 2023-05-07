<template>
  <div class="login">
  <div class="login-form">
    <form @submit.prevent="login">
      <div class="form-group">
        <label for="username">Username</label>
        <input type="text" id="username" v-model="username" required>
      </div>
      <div class="form-group">
        <label for="password">Password</label>
        <input type="password" id="password" v-model="password" required>
      </div>
      <button type="submit">Login</button>
    </form>
  </div>  
  </div>
  
</template>

<script lang="ts">
import axios from 'axios'

export default {
  data() {
    return {
      username: '',
      password: '',
    }
  },
  methods: {
    async login() {
      try {
        const response = await axios.post('http://localhost:8080/login', {
          username: this.username,
          password: this.password
        })
        localStorage.setItem('token', response.data.token)
        this.$router.push('/profile')
      } catch (error) {
        console.log(error)
      }
    }
  }
}
</script>
<style scoped>

.login {
  padding:10px;
}
.login-form {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100%;
}

form {
  background-color: #fff;
  border-radius: 8px;
  box-shadow: 0px 2px 8px rgba(0, 0, 0, 0.1);
  padding: 16px;
  max-width: 400px;
  width: 100%;
}

.form-group {
  display: flex;
  flex-direction: column;
  margin-bottom: 16px;
}

label {
  font-weight: bold;
  margin-bottom: 8px;
}

input[type="text"],
input[type="password"] {
  border: 1px solid #ccc;
  border-radius: 4px;
  font-size: 16px;
  padding: 8px;
}

button {
  background-color: #007bff;
  border: none;
  border-radius: 4px;
  color: #fff;
  cursor: pointer;
  font-size: 16px;
  padding: 8px 16px;
  transition: background-color 0.3s ease;
}

button:hover {
  background-color: #0069d9;
}
</style>
