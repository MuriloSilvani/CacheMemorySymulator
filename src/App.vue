<template>
  <div id="app">
    <div>
      <table width="100%">
        <tr>
          <th>Quantidade de blocos da memória principal</th>
          <th>Quantidade de quadros da memória cache</th>
          <th>Tamanho do bloco e quantidade de celulas</th>
        </tr>
        <tr>
          <td>
            <select v-model="memPrincipalMax">
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
          </td>
          <td>
            <select v-model="memCacheMax" :min="memPrincipalMax">
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
          </td>
          <td>
            <input v-model="tamanhoBloco" type="number" min="1" />
          </td>
        </tr>
      </table>
      <button @click="rafatorarMemoriaPrincipal()">aplicar</button>

      <table width="100%">
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
      </table>
    </div>

    <!-- listagem da memória principal -->
    <div>
      <table width="50%">
        <h1>Memória Principal</h1>
        <tr>
          <th>Bloco</th>
          <th>Endereço</th>
          <th>Dado</th>
        </tr>

        <tr v-for="(celula, index) in memPrincipal" :key="celula.endereco">
          <td v-if="index % tamanhoBloco == 0" :rowspan="tamanhoBloco">{{celula.bloco}}</td>
          <td>{{celula.endereco}}</td>
          <td>{{celula.data}}</td>
        </tr>
      </table>

      <table width="50%">
        <h1>Memória Cache</h1>
        <tr>
          <th>Rotúlo</th>
          <th>Quadro</th>
          <th v-for="celula in tamanhoBloco" :key="celula">Celula {{celula-1}}</th>
        </tr>
        <tr v-for="(quadro) in memCache" :key="quadro.quadro">
          <td>{{quadro.rotulo}}</td>
          <td>{{quadro.quadro}}</td>
          <td v-for="celula in quadro.celulas" :key="celula">{{celula}}</td>
        </tr>
      </table>
    </div>

    <!-- listagem da memória cache -->
  </div>
</template>

<script>
export default {
  data() {
    return {
      tamanhoBloco: 4,
      memPrincipal: [],
      memCache: [],

      memPrincipalMax: 256,
      memCacheMax: 16,

      enderecoBusca: "00101011",
      dadoBusca: ""
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
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
table {
  text-align: center;
  padding: 0px 50px;
  float: left;
}
td {
  border: 1px solid rgba(0, 0, 0, 0.2);
}
td:hover,
td.ativo {
  cursor: pointer;
  border: 1px solid rgba(0, 0, 0, 1);
}
</style>
