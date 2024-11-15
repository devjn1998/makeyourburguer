<template>
  <div class="main-container">
    <h1>Pedidos do Sistema</h1>
    <Dashboard />
  </div>
</template>
<script>
import Dashboard from "@/components/Dashboard.vue";

export default {
  name: "Pedidos",
  components: {
    Dashboard,
  },
};
</script>
