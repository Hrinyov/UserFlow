<template>
  <div class="profile-wrapper">
    <div class="profile">
      <div class="user-info">
        <h2>User info</h2>
        <h3>Username: {{ user.username }}</h3>
        <h3>Email: {{ user.email }}</h3>
      </div>
      <div>
        <button class="add-event" @click="viewForm()">{{ text }}</button>
        <CreateEvent v-show="formOpen" @event-created= statusMessage(true)
        @submit-error = statusMessage(false) />
      </div>
      <div class="submit-status" v-if="submitStatus.length !== 0" 
    :class="{ 'green-text': submitStatus === 'Created', 'red-text': submitStatus === 'Try again' }">
      {{ submitStatus }}

  </div>
      <div class="events-wrapper">
        <table>
          <thead>
            <tr>
              <th>Title</th>
              <th>Description</th>
              <th>Start Date</th>
              <th>End Date</th>
            </tr>
          </thead>
          <tbody>
            <tr v-if="events.length === 0">
              <td colspan="4">The event list is empty.</td>
            </tr>
            <tr v-for="event in events" :key="event.id">
              <td>{{ event.title }}</td>
              <td>{{ event.description }}</td>
              <td>{{ formatDate(event.startDate) }}</td>
              <td>{{ formatDate(event.endDate) }}</td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>
<script >
import CreateEvent from '../components/CreateEvent.vue'
import axios from 'axios'
import moment from 'moment'
export default {
  name: 'Profile',
  components: {
    CreateEvent
  },
  data() {
    return {
      formOpen: false,
      events: [],
      userId: 1,
      user: {
        username: '',
        email: ''
        },
      text: 'Create event',
      submitStatus: ''
    }
  },
  mounted() { 
    this.getUserInfoAndEvents();
  },
  methods: {
    viewForm() {
      this.formOpen = !this.formOpen
      this.formOpen ? (this.text = 'Close form') : (this.text = 'Create event')
    },
    formatDate(date) {
      return moment(date).format('DD.MM.YYYY HH:mm')
    },
    getUserInfoAndEvents(){
      this.userId = localStorage.getItem('userId');
    
      axios
      .get(`http://localhost:8080/users/${this.userId}`)
      .then((response) => {
        this.user = response.data
      })
      .catch((error) => {
        console.log(error)
      }),

      axios
      .get(`http://localhost:8080/events/${this.userId}`)
      .then((response) => {
        this.events = response.data
      })
      .catch((error) => {
        console.log(error)
      })
    },
    refresh(){
      this.getUserInfoAndEvents();
      this.viewForm();
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
  }
}
</script>
<style>
.user-info {
  display: block;
  height: 100px;
  width: 300px;
  border-bottom: 1px solid grey;
  margin: auto;
}
.add-event {
  margin: 20px;
  font-size: 20px;
}
.profile-wrapper {
  text-align: center;
}
.events-wrapper table {
  border: 2px solid black;
}
.events-wrapper td {
  border: 1px solid black;
}
.events-wrapper {
  display: flex;
  justify-content: center;
  font-size: 20px;
  margin-top: 20px;
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
