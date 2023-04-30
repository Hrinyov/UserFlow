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
      text: "",
    }
  },
  mounted(){
    this.userId = localStorage.getItem('userId');
  },
  methods: {
    createEvent() {
      this.userId = localStorage.getItem('userId');

      const newEvent = {
        title: this.title,
        description: this.description,
        startDate: this.startDate,
        endDate: this.endDate,
        userId: this.userId
      };

      axios.post('http://localhost:8080/events', {
        ...newEvent
      })
      .then(response => {
       if (response.status === 201) {
            this.$emit("event-created")
            this.clearForm()
            this.text =''
          }    
      })
      .catch(error => {
       if (error.response.status === 409){
          this.text = "You can’t create event for this time"
          this.$emit("submit-error")
       }
      
      })
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