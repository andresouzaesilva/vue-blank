<template>
  <div>
    <form @submit.prevent="cadastrar">
      <h2>Usuario</h2>
      <div class="form-group">
        <label for="nome">Nome</label>
        <input
          type="text"
          id="nome"
          class="form-control"
          required
          autofocus
          v-model="nome"
        />
      </div>
      <div class="form-group">
        <label for="senha">Senha</label>
        <textarea id="senha" class="form-control" required v-model="senha">
        </textarea>
      </div>
      <button class="btn btn-lg btn-primary btn-block" type="submit">
        Salvar
      </button>
    </form>
    <br />
    <table class="table table-striped">
      <thead>
        <tr>
          <th>Id</th>
          <th>Nome</th>
          <th>Senha</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="usuario in usuarios" :key="usuario.id">
          <td>{{ usuario.id }}</td>
          <td>{{ usuario.nome }}</td>
          <td>{{ usuario.senha }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import axios from "axios";
import { mapState } from "vuex";

export default {
  name: "usuarios",
  data() {
    return {
      nome: "",
      senha: "",
      usuarios: [],
    };
  },
  computed: {
    ...mapState(["usuario"]),
     tk: {
        get() {
            return this.$store.state.token
        },
        set(value){
            this.setToken(value);
        }
    }
  },
  methods: {
    cadastrar() {
      axios
        .post("usuario", {
          nome: this.nome,
          senha: this.senha,
        })
        .then((res) => {
          console.log(res);
          this.nome = "";
          this.senha = "";
          this.atualizar();
        })
        .catch((error) => console.log(error));
    },
    atualizar() {
      axios
        .get("usuario", { headers: { Accept: "application/json", Authorization: 'Bearer ' + this.tk} })
        .then((res) => {
          console.log(res);
          this.usuarios = res.data;
        })
        .catch((error) => console.log(error));
    },
  },
  created() {
    this.atualizar();
  },
};
</script>
