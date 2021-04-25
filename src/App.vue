<template>
  <Navigation />
  <router-view />
  <Footer />
</template>
<script>
import Navigation from "@/components/Navigation.vue";
import Footer from "@/components/Footer.vue";

export default {
  components: {
    Navigation,
    Footer,
  },
};
</script>

<style lang="scss">
/* @import '@/assets/color.scss'; */

$primary: #42b983;

body {
  margin: 0;
}
* {
  box-sizing: border-box;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #101010;
  background: #ececee;
}
.flex-center {
  display: flex !important;
  justify-content: center;
}
.flex-allcenter {
  display: flex !important;
  align-items: center;
  justify-content: center;
}
</style>
