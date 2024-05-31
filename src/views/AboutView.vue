<template>
  <div class="about">
    <h2>Lista de Pagamento</h2>

    <div>
      <div class="input-group mb-3">
        <input
          v-model="nome"
          type="text"
          class="form-control"
          aria-label="Sizing example input"
          aria-describedby="inputGroup-sizing-sm"
        />
        <div class="input-group-prepend">
          <button type="button" class="btn btn-primary" @click="salvaJogadores()">
            Adicionar +
          </button>
        </div>
      </div>
    </div>
    <table class="table table-dark">
      <thead>
        <tr>
          <th scope="col">#</th>
          <th scope="col">Nome</th>
          <th scope="col">Status</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(dado, index) in dados" :key="dado.id">
          <th scope="row">{{ index + 1 }}</th>
          <td>{{ dado.nome }}</td>
          <td>
            <button
              type="button"
              :class="buttonStatus(dado.status)"
              @click="MudaStatus(dado.id, dado.status)"
            >
              {{ labelStatus(dado.status) }}
            </button>
          </td>
        </tr>
      </tbody>
    </table>
    <br />
    <div class="input-group mb-3">
      <button type="button" class="btn btn-danger btn-block" @click="limparJogadores()">
        Limpar Jogadores
      </button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      count: 1,
      nome: '',
      dados: []
    }
  },

  methods: {
    increment() {
      return this.count++
    },

    MudaStatus(idd, status) {
      this.dados.find((dado) => dado.id == idd).status = status == 1 ? 0 : 1
      localStorage.jogadores = this.dados
    },

    labelStatus(status) {
      return status == 1 ? 'Pago' : 'Cobrar'
    },

    buttonStatus(status) {
      return status == 1 ? 'btn btn-success' : 'btn btn-danger'
    },

    salvaJogadores() {
      if (this.nome == '') {
        alert('Preencha um nome meu amigo !')
        return
      }
      var maxValue = 0
      this.dados.map((el) => {
        const valueFromObject = el.id
        maxValue = Math.max(maxValue, valueFromObject)
      })

      console.log(maxValue)

      this.dados.push({
        id: maxValue + 1,
        nome: this.nome,
        status: 0
      })
      this.nome = ''
      localStorage.jogadores = JSON.stringify(this.dados)
      console.log(localStorage.jogadores)
    },

    limparJogadores() {
      this.dados = []
      localStorage.jogadores = ''
    }
  },

  mounted() {
    if (localStorage.jogadores) {
      this.dados = JSON.parse(localStorage.jogadores)
    }
  }
}
</script>

<style>
@media (min-width: 1024px) {
  .about {
    min-height: 100vh;
    display: flex;
    align-items: center;
  }
}
</style>
