<template>
  <div class="profile">
  <div class="user-info">
    <h2>User name and info</h2>
  </div>
  <div>
    <button >Create Event</button>
  </div>
  <div class="events-wrapper">
    <h2>Tasks tabel</h2>
    <Tasks :tasks="tasks"/>

  </div>
  </div>
</template>
<script lang="ts">
import Tasks from '../components/Tasks.vue';
export default {
  name: 'Profile',
  components: {
  Tasks
  },
  data(){
    return {
      tasks: []
    }
  },
  created(){
    this.tasks = [
      {
        id: 1,
        title:"Visit doctor",
        description: "Visit doctor",
        startDate: "Jun 5st at 3:00pm",
        endDate: "Jun 5st at 4:00pm",
      },
      {
        id: 2,
        title:"Go to gym",
        description: "I need go to gym",
        startDate: "Jun 5st at 6:00pm",
        endDate: "Jun 5st at 7:00pm",
      },
      {
        id: 3,
        title:"Learn play guitar",
        description: "I go in the music scool",
        startDate: "Jun 8st at 10:00am",
        endDate: "Jun 8st at 11:00am",
      },
    ]
  }
}
</script>
