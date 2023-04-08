<template>
<h3> {{ task.title }} </h3>
<h4> {{ task.description }} </h4>
</template>
<script lang="ts">
export default { 
    name: 'Task',
    props: {
        task: Object,
    }
}
</script>