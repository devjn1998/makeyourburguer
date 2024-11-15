<template>
  <div>
    <Message :msg="msg" v-show="msg" />
    <MessageFail :msg-fail="msgFail" v-show="msgFail" />
    <form id="burger-form" @submit="postIngredients">
      <div class="input-container">
        <label class="title-style" for="nome">Nome do cliente:</label>
        <input
          type="text"
          name="nome"
          id="nome"
          v-model="nome"
          placeholder="Digite o seu nome"
        />
      </div>
      <div class="input-container">
        <label class="title-style" for="pao">Escolha o pão:</label>
        <select name="pao" id="pao" v-model="pao">
          <option v-for="pao in paes" :key="pao.id" :value="pao.tipo">
            {{ pao.tipo }}
          </option>
        </select>
      </div>
      <div class="input-container">
        <label class="title-style" for="pao"
          >Escolha a carne do seu Burger</label
        >
        <select name="carne" id="carne" v-model="carne">
          <option value=""></option>
          <option v-for="carne in carnes" :key="carne.id" :value="carne.tipo">
            {{ carne.tipo }}
          </option>
        </select>
      </div>
      <div id="opcionais-container" class="input-container">
        <label class="title-style" id="opcionais-title" for="opcionais"
          >Selecione os opcionais:</label
        >
        <div
          class="checkbox-container"
          v-for="opcional in opcionaisdata"
          :key="opcional.id"
        >
          <input
            type="checkbox"
            name="opcionais"
            id="opcionais"
            v-model="opcionais"
            :value="opcional.tipo"
          />
          <label for="salame">{{ opcional.tipo }}</label>
        </div>
        <div class="input-container">
          <input type="submit" value="Criar meu Burger" class="submit-btn" />
        </div>
      </div>
    </form>
  </div>
</template>
<script>
import { customAlphabet } from "nanoid";

import Message from "./Message.vue";
import MessageFail from "./MessageFail.vue";

export default {
  name: "BurgerForm",

  data() {
    return {
      paes: null,
      carnes: null,
      opcionaisdata: null,
      nome: null,
      pao: null,
      carne: null,
      opcionais: [],
      msg: null,
      msgFail: null,
    };
  },
  methods: {
    async getIngredients() {
      const rp = await fetch("http://localhost:3000/ingredientes");
      const data = await rp.json();

      this.paes = data.paes;
      this.carnes = data.carnes;
      this.opcionaisdata = data.opcionais;
    },

    async postIngredients(e) {
      e.preventDefault();

      this.msg = null;
      this.msgFail = null;

      if (
        !this.nome ||
        !this.pao ||
        !this.carne ||
        this.opcionais.length === 0
      ) {
        this.msgFail = "Preencha todos os campos obrigatórios!";
        return;
      }

      const nanoid = customAlphabet("0123456789", 6);

      const data = {
        id: nanoid(),
        nome: this.nome,
        pao: this.pao,
        carne: this.carne,
        opcionais: Array.from(this.opcionais),
        status: "Solicitado",
      };

      const dataJson = JSON.stringify(data);
      try {
        const rp = await fetch("http://localhost:3000/burgers", {
          method: "POST",
          headers: { "Content-Type": "application/json" },
          body: dataJson,
        });

        if (!rp.ok) {
          throw new Error("Erro na requisição", rp.statusText);
        }
        const res = await rp.json();
        this.msg = `Pedido nº ${res.id} enviado com sucesso!`;
      } catch (error) {
        this.msgFail = `Não foi possível enviar o pedido. Tente novamente mais tarde`;
      }

      this.nome = "";
      this.pao = "";
      this.carne = "";
      this.opcionais = [];
    },
  },
  mounted() {
    this.getIngredients();
  },
  components: {
    Message,
    MessageFail,
  },
};
</script>

<style scoped>
#burger-form {
  max-width: 400px;
  margin: 0 auto;
}

.input-container {
  display: flex;
  flex-direction: column;
  margin-bottom: 20px;
}

.title-style {
  font-weight: bold;
  margin-bottom: 15px;
  color: #222;
  padding: 5px 10px;
  border-left: 4px solid #fcba03;
}

label {
  font-weight: bold;
  margin-bottom: 15px;
  color: #222;
  padding-left: 5px;
}

input,
select {
  padding: 5px 10px;
  width: 300px;
}

#opcionais-container {
  flex-direction: row;
  flex-wrap: wrap;
}

#opcionais-title {
  width: 100%;
}

.checkbox-container {
  display: flex;
  align-items: flex-start;
  width: 50%;
  margin-bottom: 20px;
}

.checkbox-container span,
.checkbox-container input {
  width: auto;
  align-items: center;
}

.checkbox-container span {
  margin-left: 6px;
  font-weight: bold;
}

.submit-btn {
  background-color: #222;
  color: #fcba03;
  font-weight: bold;
  border: 2px solid #222;
  padding: 10px;
  font-size: 16px;
  margin: 0 auto;
  cursor: pointer;
  transition: 0.5s;
}
.submit-btn:hover {
  background-color: transparent;
  color: #222;
}
</style>
