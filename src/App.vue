<script setup>
import { RouterLink, RouterView } from 'vue-router'
</script>

<template>
  <header>
    <div class="wrapper">
      <nav>
        <RouterLink to="#">Settings</RouterLink>
        <RouterLink to="/">Team</RouterLink>
        <logo>UserFlow</logo>
        <RouterLink to="/profile">Profile</RouterLink>
        <RouterLink to="/login">Login</RouterLink>
      </nav>
    </div>
  </header>
  <body>
    <RouterView />
  </body>
</template>
<style scoped>
nav {
  width: 100%;
  font-size: 25px;
  text-align: center;
  margin-top: 1rem;
}

nav a {
  display: inline-block;
  padding: 0 1rem;
  border-left: 1px solid var(--color-border);
}

nav a:first-of-type {
  border: 0;
}
</style>
