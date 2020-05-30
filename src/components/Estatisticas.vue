<template>
  <div class="col-md-4">
    <ul class="list-group">
      <li class="list-group-item d-flex justify-content-between align-items-center">
        Numero de Acessos
        <span class="badge badge-primary badge-pill">{{NumerodeAcessos}}</span>
      </li>
      <li class="list-group-item d-flex justify-content-between align-items-center">
        Numero de Acertos
        <span class="badge badge-primary badge-pill">{{NumerodeAcertos}}</span>
      </li>
      <li class="list-group-item d-flex justify-content-between align-items-center">
        Numero de Faltas
        <span class="badge badge-primary badge-pill">{{NumerodeFaltas}}</span>
      </li>
      <li class="list-group-item d-flex justify-content-between align-items-center">
        Numero de Leituras
        <span class="badge badge-primary badge-pill">{{NumerodeLeituras}}</span>
      </li>
      <li class="list-group-item d-flex justify-content-between align-items-center">
        Numero de Escritas
        <span class="badge badge-primary badge-pill">{{NumerodeEscritas}}</span>
      </li>
      <li class="list-group-item d-flex justify-content-between align-items-center">
        Numero de Acertos na Leitura
        <span
          class="badge badge-primary badge-pill"
        >{{NumerodeAcertosnaLeitura}}</span>
      </li>
      <li class="list-group-item d-flex justify-content-between align-items-center">
        Numero de Acertos na Escrita
        <span
          class="badge badge-primary badge-pill"
        >{{NumerodeAcertosnaEscrita}}</span>
      </li>
      <li class="list-group-item d-flex justify-content-between align-items-center">
        Numero de Faltas na Leitura
        <span
          class="badge badge-primary badge-pill"
        >{{NumerodeFaltasnaLeitura}}</span>
      </li>
      <li class="list-group-item d-flex justify-content-between align-items-center">
        Numero de Faltas na Escrita
        <span
          class="badge badge-primary badge-pill"
        >{{NumerodeFaltasnaEscrita}}</span>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: "Estatisticas",
  props: {
    NumerodeAcessos: Number,
    NumerodeAcertos: Number,
    NumerodeFaltas: Number,
    NumerodeLeituras: Number,
    NumerodeEscritas: Number,
    NumerodeAcertosnaLeitura: Number,
    NumerodeAcertosnaEscrita: Number,
    NumerodeFaltasnaLeitura: Number,
    NumerodeFaltasnaEscrita: Number
  }
};
</script>

<style scoped>
li span {
  font-size: 18px;
}
</style>