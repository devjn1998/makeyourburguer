<template>
  <Navbar :logo="logo_src" :alt="alt_src" />
  <router-view />
  <Footer />
</template>
<script>
import Footer from "./components/Footer.vue";
import Navbar from "./components/Navbar.vue";

export default {
  components: {
    Navbar,
    Footer,
  },
  data() {
    return {
      logo_src: "img/logo.png",
      alt_src: "Make Your Burger",
    };
  },
};
</script>

<style>
* {
  font-family: "Helvetica", sans-serif;
  margin: 0px;
  padding: 0px;
  box-sizing: border-box;
}

.main-container {
  margin: 50px;
  min-height: 250px;
}

h1 {
  color: #222;
  text-align: center;
  font-size: 40px;
  margin-bottom: 30px;
}
</style>
