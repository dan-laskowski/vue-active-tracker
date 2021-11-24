<template>
  <div class="min-h-full font-Poppins box-border">
    <Navigation />
    <router-view />
  </div>
</template>

<script>
import Navigation from "./components/Navigation.vue";
export default {
  components: {
    Navigation,
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Poppins:ital,wght@0,100;0,300;0,400;0,500;0,600;0,700;1,100&display=swap");
</style>
