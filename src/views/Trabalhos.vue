<template>
  <div>
    <form @submit.prevent="cadastrar">
      <h2 class="text-center titulo">Trabalhos</h2>
      <div v-if="usuario" class="form-group">
        <label for="titulo">Titulo</label>
        <input type="text" id="titulo"
            class="form-control" required autofocus
            v-model="titulo">
      </div>
      <div v-if="usuario" class="form-group">
        <label for="texto">Texto</label>
        <textarea id="texto"
            class="form-control" required
            v-model="texto">
        </textarea>
      </div>
      <button v-if="usuario" class="btn btn-lg btn-primary btn-block" type="submit">Salvar</button>
    </form>
    <br>
    <table class="table table-striped">
      <thead>
        <tr>
          <th>Id</th>
          <th>Titulo</th>
          <th>Texto</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="ant in trabalhos" :key="ant.id">
          <td>{{ ant.id }}</td>
          <td>{{ ant.titulo }}</td>
          <td>{{ ant.texto }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import axios from 'axios'
import { mapState } from 'vuex'

export default {
  name: 'trabalhos',
  data() {
    return {
      titulo: '',
      texto: '',
      trabalhos: []
    }
  },
  computed: {
    ...mapState([
      'usuario',
      'token'
    ])
  },
  methods: {
    cadastrar() {
      axios.post('trabalho',
          {
            titulo: this.titulo,
            texto: this.texto,
          })
        .then(res => {
          console.log(res);
          this.titulo = '';
          this.texto = '';
          this.atualizar();
        })
        .catch(error => console.log(error))
    },
    atualizar () {
      axios.get('/trabalho', 
          { headers: { Accept: 'application/json' } })
        .then(res => {
          console.log(res)
          this.trabalhos = res.data
        })
        .catch(error => console.log(error))
    }
  },
  created () {
    this.atualizar()
  }
}
</script>

<style>
body {
  background: #94D5B1;
  padding: 10px;
  font-family: 'Work Sans';
}
.titulo{
  margin-top: 5%;
  font-size: 25px;
  color: #ffffff;
  font-weight: bold;
}

</style>