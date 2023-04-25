<template>
  <div class="form-wrapper">
  <form @submit.prevent="createEvent">
    <label for="title">Title:</label>
    <input type="text" id="title" v-model="title" required>
    <br>
    <label for="description">Description:</label>
    <textarea id="description" v-model="description" required></textarea>
    <br>
    <label for="startDate">Start Date:</label>
    <input type="datetime-local" id="startDate" v-model="startDate" required>
    <br>
    <label for="endDate">End Date: </label>
    <input type="datetime-local" id="endDate" v-model="endDate" required>
    <br>
    <button type="submit">Create Event</button>
    <span>{{ text }}</span>
  </form>
  </div>
</template>

<script>
import axios from 'axios';
export default {
  data() {
    return {
      title: '',
      description: '',
      startDate: '',
      endDate: '',
      userId: '',
      events: [],
      text: "",
    }
  },
  mounted(){
    this.userId = localStorage.getItem('userId');
  },
  methods: {
    async createEvent() {
    this.userId = localStorage.getItem('userId');

    const newEvent = {
      title: this.title,
      description: this.description,
      startDate: this.startDate,
      endDate: this.endDate,
      userId: this.userId
    };

    try {
    const response = await axios.get(`http://localhost:8080/events/${this.userId}`);
    this.events = response.data;
    if (this.events.length === 0) {
      console.log('problem');
    }
    console.log(this.events)
    let conflictingEvents = this.events.filter(event => {
      return (event.startDate <= newEvent.endDate && event.endDate >= newEvent.startDate);
    });
    if (conflictingEvents.length > 0) {
      this.text = "You can't create event for this time.";
      conflictingEvents = '';
    } else {
      console.log('Created');
      axios.post('http://localhost:8080/events', {
        ...newEvent
      })
      .then(response => {
        this.$emit('event-created');
      })
      .catch(error => {
        console.log(error);
      });
      this.clearForm();
      this.text = '';
      }
      } catch (error) {
    console.log(error);
      }
    },
    clearForm(){
      this.title = '';
      this.description = '';
      this.startDate = '';
      this.endDate = '';
    }

  }
}

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