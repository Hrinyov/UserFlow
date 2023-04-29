<template>
  <div class="main-wrapper">
  <div class="main">
    <button class="add-user" @click="veiwForm()">{{ text }}</button>
    <CreateUser v-show="formOpen" @form-submitted=statusMessage(true)
    @submit-error = statusMessage(false)
    />
  </div>
  <div class="submit-status" v-if="submitStatus.length !== 0" 
    :class="{ 'green-text': submitStatus === 'Created', 'red-text': submitStatus === 'Try again' }">
      {{ submitStatus }}

  </div>
  <div class="users-table">
<table>
  <thead>
    <tr>
      <th>Username</th>
      <th>Email</th>
      <th>Phone Number</th>
      <th>Events Count</th>
      <th>Next Event Date</th>
    </tr>
  </thead>
  <tbody>
    <tr v-if="users.length === 0">
              <td colspan="6">The users list is empty.</td>
            </tr>
    <tr class="user-tr" v-for="user in users" :key="user.id" 
    :class="{ 'selected': selectedIndex == user.id }" @click="selectUser(user)">
      <td>{{ user.username }}</td>
      <td>{{ user.email }}</td>
      <td>{{ user.phonenumber }}</td>
      <td>{{ user.events.length }}</td>
      <td>{{ formatDate(nextEventDate(user)) }}</td>
    </tr>
  </tbody>
  
</table>
  </div>
  <h4 class="info">***marked in GREEN user is displayed in the profile***</h4>
  </div>
  
</template>
<script >

import axios from 'axios';
import CreateUser from '../components/CreateUser.vue';
import moment from 'moment';

export default {
  data() {
    return {
      formOpen: false,
      users: [],
      text: 'Create user',
      defaultUserId: 1,
      selectedIndex: localStorage.getItem('userId'),
      submitStatus: '',
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
    veiwForm() {
      this.formOpen = !this.formOpen;
      this.formOpen ? this.text = 'Close form' : this.text = 'Create user'
    },
    selectUser(user) {
      this.user = user.id;
      localStorage.setItem('userId', user.id);
      this.selectRow(user.id)
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
    nextEventDate(user){
      const now = new Date();
      let nextEventDay = null;

      user.events.forEach((event) => {
      const startDate = new Date(event.startDate);

      if (startDate >= now && (!nextEventDay || startDate < nextEventDay)) {
        nextEventDay = startDate;
      }
      });
      return nextEventDay;
     },
    formatDate(date) {
      if(date === null){
      return '-'
      } else 
      return moment(date).format('YYYY.MM.DD HH:mm')
    },
    refresh(){
      this.getUsers();
      this.veiwForm();
      },
    selectRow(index) {
      if (this.selectedIndex !== index) {
        this.selectedIndex = index;
      }
    },
    statusMessage(status){
      if(status){
        this.submitStatus = "Created"
        this.refresh()
      } else {
        this.submitStatus = "Try again"
      }
      setTimeout(() => {
              this.submitStatus = '';
            }, 3000);
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
.user-tr.selected {
  background-color: #21b77b57;
}
.info {
margin-top: 5px;
color: #21b77b;;
}
.submit-status{
  font-size:21px;
  font-weight: 600;
}
.green-text{
  color:#21b77b;
}
.red-text{
  color:rgb(209, 37, 37);
}
</style>
