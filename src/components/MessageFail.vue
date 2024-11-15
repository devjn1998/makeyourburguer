<template>
  <div class="container-message-fail" v-show="visible">
    <p>{{ msgFail }}</p>
  </div>
</template>
<script>
export default {
  name: "MessageFail",
  props: {
    msgFail: String,
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
.container-message-fail {
  display: flex;
  justify-content: center;
  border-radius: 5px;
  padding: 10px;
  max-width: 400px;
  margin: 30px auto;
  text-align: center;
  color: #ea2027;
  background-color: #ff7979;
  border: 2px solid #eb4d4b;
}
</style>
