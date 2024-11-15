<template>
  <div id="main-banner">
    <h1>Make Your Burger</h1>
  </div>
</template>
<script scoped>
export default {
  name: "Banner",
};
</script>
<style scoped>
#main-banner {
  background-image: url("/public/img/burger.jpg");
  background-position: 0 -350px;
  height: 700px;
  width: 100%;
  background-size: cover;
  display: flex;
  justify-content: flex-start;
  align-items: center;
}

#main-banner h1 {
  color: #fff;
  font-size: 60px;
  text-align: center;
  background-color: #222;
  padding: 20px 40px;
}
</style>
