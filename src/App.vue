<template>
  <div id="app">
    <div class="container-fluid m-0 p-0">
      <div class="row m-0 p-0">
        <div class="col-md-4">
          <div class="input-group mb-3">
            <div class="col-md-12">
              <label>Quantidade de blocos na memória principal</label>
            </div>
            <select v-model="memPrincipalMax" class="custom-select">
              <option value="2">2</option>
              <option value="4">4</option>
              <option value="8">8</option>
              <option value="16">16</option>
              <option value="32">32</option>
              <option value="64">64</option>
              <option value="128">128</option>
              <option value="256">256</option>
              <option value="512">512</option>
              <option value="1024">1024</option>
            </select>
          </div>
          <div class="input-group mb-3">
            <div class="col-md-12">
              <label>Quantidade de quadros na memória cache</label>
            </div>
            <select v-model="memCacheMax" class="custom-select">
              <option value="2">2</option>
              <option value="4">4</option>
              <option value="8">8</option>
              <option value="16">16</option>
              <option value="32">32</option>
              <option value="64">64</option>
              <option value="128">128</option>
              <option value="256">256</option>
              <option value="512">512</option>
              <option value="1024">1024</option>
            </select>
          </div>
          <div class="input-group mb-3">
            <div class="col-md-12">
              <label>Tamanho dos blocos e celulas</label>
            </div>
            <select v-model="memCacheMax" class="custom-select">
              <option value="2">2</option>
              <option value="4">4</option>
              <option value="8">8</option>
              <option value="16">16</option>
              <option value="32">32</option>
              <option value="64">64</option>
              <option value="128">128</option>
              <option value="256">256</option>
              <option value="512">512</option>
              <option value="1024">1024</option>
            </select>
          </div>
        </div>
        <div class="col-md-4"></div>
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
        <!-- <table width="100%">
        </tr>
      </table>
        <button @click="rafatorarMemoriaPrincipal()">aplicar</button>-->

        <!-- <table width="100%">
        <h1>Write Read</h1>
        <tr>
          <th>Endereço</th>
          <th>Dado</th>
          <th>Buscar</th>
        </tr>
        <tr>
          <td>
            <input v-model="enderecoBusca" />
          </td>
          <td>
            <input v-model="dadoBusca" />
          </td>
          <td>
            <button @click="buscar()">buscar</button>
          </td>
        </tr>
        </table>-->
      </div>

      <div class="row m-0 p-0 mt-5 text-center">
        <div class="col-md-6 m-0 p-0">
          <h1 class="mb-4">Memória Principal</h1>

          <table class="table table-dark table-hover m-0 p-0">
            <thead>
              <tr>
                <th>Bloco</th>
                <th>Endereço</th>
                <th>Dado</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="(celula, index) in memPrincipal" :key="celula.endereco">
                <td
                  class="align-middle"
                  v-if="index % tamanhoBloco == 0"
                  :rowspan="tamanhoBloco"
                >{{celula.bloco}}</td>
                <td>{{celula.endereco}}</td>
                <td>{{celula.data}}</td>
              </tr>
            </tbody>
          </table>
        </div>
        <div class="col-md-6 m-0 p-0">
          <h1 class="mb-4">Memória Cache</h1>
          <table class="table table-hover m-0 p-0">
            <thead>
              <tr>
                <th>Rotúlo</th>
                <th>Quadro</th>
                <th v-for="celula in tamanhoBloco" :key="celula">Celula {{celula-1}}</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="(quadro) in memCache" :key="quadro.quadro">
                <td>{{quadro.rotulo}}</td>
                <td>{{quadro.quadro}}</td>
                <td v-for="celula in quadro.celulas" :key="celula">{{celula}}</td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>

      <!-- listagem da memória cache -->
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      tamanhoBloco: 4,
      memPrincipal: [],
      memCache: [],

      memPrincipalMax: 4,
      memCacheMax: 16,

      enderecoBusca: "00101011",
      dadoBusca: "",

      NumerodeAcessos: 0,
      NumerodeAcertos: 0,
      NumerodeFaltas: 0,
      NumerodeLeituras: 0,
      NumerodeEscritas: 0,
      NumerodeAcertosnaLeitura: 0,
      NumerodeAcertosnaEscrita: 0,
      NumerodeFaltasnaLeitura: 0,
      NumerodeFaltasnaEscrita: 0
    };
  },
  methods: {
    rafatorarMemoriaPrincipal() {
      this.memPrincipal = [];
      this.popularMemoriaPrincipal();
    },
    popularMemoriaPrincipal() {
      // Iniciar memória principal
      // this.memPrincipal = [];
      this.memPrincipal = [];

      let enderecoCelula = 0;
      let blocoMemoriaPrincipal = 0;

      let auxString = "";
      const auxTamanhoString = Math.log(this.memPrincipalMax) / Math.log(2);
      while (auxString.length < auxTamanhoString) {
        auxString += "0";
      }

      while (this.memPrincipalMax * 4 > this.memPrincipal.length) {
        this.memPrincipal.push({
          endereco: (auxString + enderecoCelula.toString(2)).slice(
            0 - auxTamanhoString
          ),
          bloco: blocoMemoriaPrincipal,
          data: this.randomValue()
        });
        enderecoCelula++;
        if (enderecoCelula % this.tamanhoBloco == 0) {
          blocoMemoriaPrincipal++;
        }
      }
    },
    popularMemoriaCache() {
      this.memPrincipal = [];
    },
    randomValue() {
      return Math.random()
        .toString(36)
        .substring(7);
    },
    buscar() {
      let endereco = this.enderecoBusca;
      let rotulo = endereco.substring("", 2);
      let quadro = endereco.substring(2, 6);
      let celula = endereco.substring(6, 8);

      this.memCache[parseInt(quadro, 2)].rotulo = rotulo;

      if (parseInt(celula, 2) == 1) {
        celula = (parseInt(celula, 2) - 1).toString(2);
      }
      if (parseInt(celula, 2) == 2) {
        celula = (parseInt(celula, 2) - 2).toString(2);
      }
      if (parseInt(celula, 2) == 3) {
        celula = (parseInt(celula, 2) - 3).toString(2);
      }

      for (let i = 0; i < this.tamanhoBloco; i++) {
        this.memCache[parseInt(quadro, 2)].celulas[
          parseInt(celula, 2) + i
        ] = this.memPrincipal[parseInt(endereco, 2) + i].data;
      }
    }
  },
  mounted: function() {
    this.popularMemoriaPrincipal();

    // Iniciar memória cache
    let blocoMemoriaPrincipal = 0;

    while (this.memCacheMax > this.memCache.length) {
      let celulas = [];
      while (celulas.length < this.tamanhoBloco) {
        celulas.push("");
      }

      this.memCache.push({
        rotulo: "",
        quadro: blocoMemoriaPrincipal,
        celulas
      });
      blocoMemoriaPrincipal++;
    }
  }
};
</script>

<style>
body {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
