<template>
  <div class="main-wrapper">
  <div class="main">
    <button class="add-user" @click="VeiwForm()">{{ text }}</button>
    <CreateUser v-show="formOpen" @form-submitted="refresh"/>
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
    <tr class="user-tr" v-for="user in users" :key="user.id"  @click="selectUser(user)">
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
      text: 'Create user',
      defaultUserId: 1,
    }
  },
  components: {
  CreateUser
  },
  mounted() {
  this.checkUserId();
  this.getUsers();
  },
  methods: {
    VeiwForm() {
    this.formOpen = !this.formOpen;
    this.formOpen ? this.text = 'Close form' : this.text = 'Create user'
    },
    selectUser(user) {
      this.user = user.id;
      localStorage.setItem('userId', user.id);
    },
    checkUserId(){
      const userId = localStorage.getItem('userId');
      if(userId === null){
      localStorage.setItem('userId', 1);
      }
    },
    getUsers(){
    axios.get('http://localhost:8080/users')
      .then(response => {
      this.users = response.data;
      })
      .catch(error => {
     console.log(error);
      });
      },
      refresh(){
        this.getUsers();
        this.VeiwForm();
      }
    },
  
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
.user-tr:hover {
background-color: hsla(198, 71%, 55%, 0.2);
}
.user-tr {
  cursor: pointer;
}
.users-table {
  display: flex;
  justify-content: center;
  font-size: 20px;
  margin-top: 20px;
}
</style>
