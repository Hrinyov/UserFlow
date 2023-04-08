<template>
  <div class="main">
    <button class="add-user" @click="VeiwForm()">Create user</button>
  </div>
</template>
<script >
export default {
  methods: {
    VeiwForm(){
      console.log('form');
    }
  }
}
</script>
<style>
/* .add-user {
  margin-top: 20px;
  background-color:rgb(119, 210, 103);
  cursor: pointer;
}
.add-user:hover {
  border: 3px solid black;
}
.add-user:active {
  background-color: rgb(93, 164, 80);
} */
</style>
