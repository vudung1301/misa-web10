<template>
  <div>
    <the-header></the-header>
    <the-body></the-body>
    
  </div>
</template>

<script>
import TheHeader from "./components/layout/TheHeader.vue";
import TheBody from "./components/layout/TheBody.vue";
export default {
  name: "App",
  components: {
    TheHeader,
    TheBody,
  },
  
};
</script>

<style>
@import url(./css/main.css);
</style>
