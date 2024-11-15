<template>
  <div id="burger-table" v-if="burgers">
    <Message ref="messageComponent" :msg="msg" v-show="msg" />
    <MessageFail :msg-fail="msgFail" v-show="msgFail" />
    <div>
      <div id="burger-table-heading">
        <div class="order-id">#:</div>
        <div>Cliente</div>
        <div>Pão:</div>
        <div>Carne:</div>
        <div>Opcionais:</div>
        <div>Ações:</div>
      </div>
      <div id="burger-table-rows">
        <div
          class="burger-table-row"
          v-for="burger in burgers"
          :key="burger.id"
        >
          <div class="order-number">{{ burger.id }}</div>
          <div>{{ burger.nome }}</div>
          <div>{{ burger.pao }}</div>
          <div>{{ burger.carne }}</div>
          <div>
            <div>
              <ul>
                <li v-for="(opcional, index) in burger.opcionais" :key="index">
                  {{ opcional }}
                </li>
              </ul>
            </div>
          </div>
          <div>
            <select
              name="status"
              class="status"
              @change="updateStatus($event, burger.id)"
            >
              <option
                v-for="s in status"
                :value="s.tipo"
                :key="s.id"
                :selected="s.tipo === burger.status"
              >
                {{ s.tipo }}
              </option>
            </select>
            <button @click="deletePedido(burger.id)" class="delete-btn">
              Cancelar
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
  <div v-else>
    <h2>Não há pedidos no momento</h2>
  </div>
</template>
<script>
import Message from "./Message.vue";
import MessageFail from "./MessageFail.vue";

export default {
  name: "Dashboard",
  data() {
    return {
      burgers: null,
      burger_id: null,
      status: [],
      msg: null,
      msgFail: null,
    };
  },
  methods: {
    async getPedidos() {
      const res = await fetch("http://localhost:3000/burgers");

      const data = await res.json();

      this.burgers = data;

      this.getStatus();
    },

    async getStatus() {
      const res = await fetch("http://localhost:3000/status");
      const data = await res.json();
      this.status = data;
    },

    async updateStatus(event, id) {
      const option = event.target.value;
      const dataJson = JSON.stringify({
        status: option,
      });

      const req = await fetch(`http://localhost:3000/burgers/${id}`, {
        method: "PATCH",
        headers: { "Content-Type": "application/json" },
        body: dataJson,
      });

      const res = await req.json();

      this.msg = `Pedido  Nº ${res.id} foi atualizado para ${res.status}!`;
      console.log(res);
    },

    async deletePedido(id) {
      try {
        const res = await fetch(`http://localhost:3000/burgers/${id}`, {
          method: "DELETE",
          headers: { "Content-Type": "application/json" },
        });

        if (res.ok) {
          this.burgers = this.burgers.filter((burger) => burger.id !== id);
          this.msg = `Pedido ${id} deletado com sucesso!`;
        } else {
          this.msgFail = `Erro ao deletar o pedido ${this.id}!`;
        }
      } catch (error) {
        this.msgFail = `Erro de conexão com o servidor! Tente novamente mais tarde!`;
      }
    },
  },
  mounted() {
    this.getPedidos();
  },

  components: {
    Message,
    MessageFail,
  },
};
</script>
<style scoped>
#burger-table {
  max-width: 1200px;
  margin: 0 auto;
}

#burger-table-heading,
#burger-table-rows,
.burger-table-row {
  display: flex;
  flex-wrap: wrap;
  align-items: center;
}

#burger-table-heading {
  font-weight: bold;
  padding: 12px;
  border-bottom: 2px solid #333;
}

#burger-table-heading div,
.burger-table-row div {
  width: 19%;
}

.burger-table-row {
  width: 100%;
  padding: 12px;
  border-bottom: 1px solid #ccc;
}

#burger-table-heading .order-id,
.burger-table-row .order-number {
  width: 5%;
}

select {
  padding: 12px 6px;
  margin-right: 12px;
}

.delete-btn {
  background-color: #222;
  color: #fcba03;
  font-weight: bold;
  border: 2px solid #222;
  padding: 10px;
  font-size: 16px;
  margin: 0 auto;
  cursor: pointer;
  transition: 0.5;
}

.delete-btn:hover {
  background-color: transparent;
}

ul {
  list-style: none;
}
</style>
