<template>
  <HUD_Animations/>
  <div>working</div>
</template>

<script>
import HUD_Animations from "./components/HUD-Animations.vue";

export default {
  name: 'App',
  components: {
    HUD_Animations
  }
}
</script>

<style>
body {
  margin: 0;
  font-family: 'Montserrat', sans-serif;
  overflow: hidden;
}

#app {
  height: 100vh;
  background: url(./assets/gta2.1.svg) no-repeat center bottom fixed;
  -webkit-background-size: cover;
  -moz-background-size: cover;
  -o-background-size: cover;
  background-size: cover;
}
</style>
