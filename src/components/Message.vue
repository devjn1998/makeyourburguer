<template>
  <div class="container-message" v-show="visible">
    <p>{{ msg }}</p>
  </div>
</template>
<script>
export default {
  name: "Message",
  props: {
    msg: String,
  },
  data() {
    return {
      visible: true,
    };
  },

  watch: {
    msg(newVal) {
      if (newVal) {
        this.temporizador();
        this.visible = true;
      }
    },
  },

  methods: {
    temporizador() {
      setTimeout(() => {
        this.visible = false;
      }, 3000);
    },
  },
};
</script>
<style scoped>
.container-message {
  display: flex;
  justify-content: center;
  border-radius: 5px;
  padding: 10px;
  max-width: 400px;
  margin: 30px auto;
  text-align: center;
  color: #0984e3;
  background-color: #dff9fb;
  border: 2px solid #c7ecee;
}
</style>
