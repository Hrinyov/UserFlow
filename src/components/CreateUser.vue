<template>
  <div class="form-wrapper">
    <form @submit.prevent="handleSubmit">
      <label>
        First name:
        <input type="text" v-model="firstName" required>
      </label>
      <label>
        Last name:
        <input type="text" v-model="lastName" required>
      </label>
      <label>
        Email:
        <input type="email" v-model="email" required>
      </label>
      <label>
        Phone number:
        <input type="tel" v-model="phoneNumber" required>
      </label>
      <button type="submit">Create User</button>
    </form>
  </div>
</template>

<script>
export default {
  data() {
    return {
      firstName: '',
      lastName: '',
      email: '',
      phoneNumber: '',
    };
  },
  methods: {
    handleSubmit() {
      const userData = {
        firstName: this.firstName,
        lastName: this.lastName,
        email: this.email,
        phoneNumber: this.phoneNumber,
      };
      console.log(userData); 
    },
  },
};
</script>

<style scoped>
.form-wrapper {
  border: 2px solid black;
  padding: 10px;
  width: 300px;
  margin: 0 auto;
}

form label {
  display: block;
  margin-bottom: 6px;
}

form input {
  width: 100%;
  padding: 5px;
  margin-bottom: 5px;
  box-sizing: border-box;
}

button[type="submit"] {
  display: block;
  margin: 10px auto;

}
</style>