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
      <span>{{ text }}</span>
    </form>
  </div>
</template>

<script>
import axios from 'axios';
export default {
  data() {
    return {
      firstName: '',
      lastName: '',
      email: '',
      phoneNumber: '',
      text: '',
    };
  },
  methods: {
    handleSubmit() {
      if(this.validateForm()){
       const userData = {
        username: this.firstName +" "+ this.lastName,
        email: this.email,
        phonenumber: this.phoneNumber,
      };
      axios.post('http://localhost:8080/users', {
      ...userData
      })
      .then(response => {
       if (response.status === 201) {
            this.$emit("form-submitted")
            this.clearForm()
            this.text =''
          }    
      })
      .catch(error => {
       if (error.response.status === 409){
          this.text = 'User with this username already exists'
          this.$emit("submit-error")
       }
      
      })
      }
    },
    clearForm(){
      this.firstName = '';
      this.lastName = '';
      this.email = '';
      this.phoneNumber = '';
    },
    validateForm(){
      const nameRegex = /^[a-zA-Z]+$/;
      const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
      const phoneNumberRegex = /^\d{10}$/;
      if (
      !this.firstName ||
      !nameRegex.test(this.firstName) ||
      !this.lastName ||
      !nameRegex.test(this.lastName) ||
      !this.email ||
      !emailRegex.test(this.email) ||
      !this.phoneNumber || 
      !phoneNumberRegex.test(this.phoneNumber)
        ){
          alert('Please fill out all the form fields with valid data');
          return false;
        } else {
      return true;
        }
    }
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
span {
color: #e12020;
}
</style>