<script setup lang="ts">
// This starter template is using Vue 3 <script setup> SFCs
// Check out https://v3.vuejs.org/api/sfc-script-setup.html#sfc-script-setup
import SimpleTimer from "./components/SimpleTimer.vue";
</script>

<template>
  <SimpleTimer />
</template>

<style>
@import url("https://fonts.googleapis.com/css2?family=Roboto:ital,wght@0,400;1,900&display=swap");

:root {
  --clr-dark: #262626;
  --clr-light: #fefefe;
  --clr-primary: #e20074;
  --fw-normal: 400;
  --fw-bold: 900;
}

body {
  background: var(--clr-dark);
  margin: 0;
}
*,
*::after,
*::before {
  box-sizing: border-box;
}

#app {
  font-family: "Roboto", sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: var(--clr-light);
}
</style>
