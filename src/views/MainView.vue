<template>
  <div class="main-wrapper">
  <div class="main">
    <button class="add-user" @click="VeiwForm()">{{ text }}</button>
    <CreateUser v-show="formOpen"/>
  </div>
  <div class="users-table">
<table>
  <thead>
    <tr>
      <th>Username</th>
      <th>Email</th>
      <th>Phone Number</th>
    </tr>
  </thead>
  <tbody>
    <tr v-for="user in users" :key="user.id">
      <td>{{ user.username }}</td>
      <td>{{ user.email }}</td>
      <td>{{ user.phonenumber }}</td>
    </tr>
  </tbody>
</table>
  </div>
  </div>

</template>
<script >

import axios from 'axios';
import CreateUser from '../components/CreateUser.vue';
export default {
  data() {
    return {
      formOpen: false,
      users: [],
      text: 'Create user'
    }
  },
  components: {
  CreateUser
  },
  mounted() {
  axios.get('http://localhost:8080/users')
  .then(response => {
    this.users = response.data;
  })
  .catch(error => {
    console.log(error);
  });
  },
  methods: {
    VeiwForm() {
    this.formOpen = !this.formOpen;
    this.formOpen ? this.text = 'Close form' : this.text = 'Create user'
    }
  }
}
{

}
</script>
<style>
.add-user {
  margin: 20px;
  font-size: 20px;
}
.main-wrapper {
  text-align: center;
}
.main {
  display: block;
}
.users-table table {
  border: 2px solid black;

}
.users-table td {
  border: 1px solid black;
}
.users-table {
  display: flex;
  justify-content: center;
  font-size: 20px;
  margin-top: 20px;
}
</style>
