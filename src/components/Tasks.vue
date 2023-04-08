<template>
<div  :key="task.id" v-for="task in tasks">
<Task :task="task"/>
</div>
</template>

<script lang="ts">
import Task from '../components/Task.vue'
export default {
name: "Tasks",
props: {
    tasks: Array
},
components: {
    Task
}
}
</script>