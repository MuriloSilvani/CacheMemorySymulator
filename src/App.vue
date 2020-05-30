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
            <select v-model="tamanhoBloco" class="custom-select" @change="refactorSizes()">
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
          <div class="col-md-12 m-0 p-0">
            <div
              class="alert alert-danger"
            >Zera todas as estatisticas e dados salvos para gerar as memórias com os novos tamanhos</div>
            <button
              @click="refactorSizes()"
              class="btn btn-outline-danger btn-md btn-block"
            >Aplicar alterações</button>
          </div>
        </div>
        <Estatisticas
          :NumerodeAcessos="NumerodeAcessos"
          :NumerodeAcertos="NumerodeAcertos"
          :NumerodeFaltas="NumerodeFaltas"
          :NumerodeLeituras="NumerodeLeituras"
          :NumerodeEscritas="NumerodeEscritas"
          :NumerodeAcertosnaLeitura="NumerodeAcertosnaLeitura"
          :NumerodeAcertosnaEscrita="NumerodeAcertosnaEscrita"
          :NumerodeFaltasnaLeitura="NumerodeFaltasnaLeitura"
          :NumerodeFaltasnaEscrita="NumerodeFaltasnaEscrita"
        />
        <div class="col-md-4">
          <div class="input-group mb-3">
            <div class="col-md-12">
              <label>Endereço</label>
            </div>
            <input v-model="enderecoBusca" class="form-control" />
          </div>
          <div class="input-group mb-3">
            <div class="col-md-12">
              <label>Dado</label>
            </div>
            <input v-model="dadoBusca" class="form-control" />
          </div>
          <div class="input-group mb-3">
            <div class="col-md-6">
              <input
                :disabled="!(enderecoBusca != '')"
                @click="read()"
                class="btn btn-outline-primary btn-lg btn-block"
                type="button"
                value="Read"
              />
            </div>
            <div class="col-md-6">
              <input
                :disabled="!(enderecoBusca != '' && dadoBusca != '')"
                @click="write()"
                class="btn btn-outline-primary btn-lg btn-block"
                type="button"
                value="Write"
              />
            </div>
          </div>
        </div>
      </div>

      <div class="row m-0 p-0 mt-5 text-center">
        <div class="col-md-6 m-0 p-0">
          <h1 class="mb-4 text-primary">Memória Principal</h1>

          <table class="table table-dark table-hover m-0 p-0">
            <thead>
              <tr>
                <th>Bloco</th>
                <th>Endereço</th>
                <th>Dado</th>
              </tr>
            </thead>
            <tbody>
              <tr
                v-for="(celula, index) in memPrincipal"
                :key="celula.endereco"
                @click="selecionaEndereco(celula.endereco)"
              >
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
          <h1 class="mb-4 text-primary">Memória Cache</h1>
          <table class="table table-hover m-0 p-0">
            <thead>
              <tr>
                <th>Rotúlo</th>
                <th>Quadro</th>
                <th v-for="celula in tamanhoBloco" :key="celula">Celula {{celula}}</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="(quadro) in memCache" :key="quadro.quadro">
                <td>{{quadro.rotulo}}</td>
                <td>{{quadro.quadro}}</td>
                <td v-for="(celula, index) in quadro.celulas" :key="index">{{celula}}</td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>
    </div>
  </div>
</template>



<script>
import Estatisticas from "./components/Estatisticas";
export default {
  name: "App",
  components: {
    Estatisticas
  },
  data() {
    return {
      tamanhoBloco: 4,
      memPrincipal: [],
      memCache: [],

      memPrincipalMax: 256,
      memCacheMax: 16,

      enderecoBusca: "",
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
    popularMemoriaPrincipal() {
      // Iniciar memória principal
      this.memPrincipal = [];

      let enderecoCelula = 0;
      let blocoMemoriaPrincipal = 0;

      let auxString = "";
      const auxTamanhoString =
        Math.log(this.memPrincipalMax * this.tamanhoBloco) / Math.log(2);
      while (auxString.length < auxTamanhoString) {
        auxString += "0";
      }

      while (
        this.memPrincipalMax * this.tamanhoBloco >
        this.memPrincipal.length
      ) {
        this.memPrincipal.push({
          endereco: (auxString + enderecoCelula.toString(2)).slice(
            0 - auxString.length
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
      this.memCache = [];

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
    },
    randomValue() {
      return Math.random()
        .toString(36)
        .substring(7);
    },
    selecionaEndereco(endereco) {
      this.enderecoBusca = endereco;
    },
    writeOnCache(endereco, rotulo, quadro, celula) {
      this.memCache[quadro].rotulo = rotulo;

      if (celula == 1) {
        celula = celula - 1;
        endereco = endereco - 1;
      }
      if (celula == 2) {
        celula = celula - 2;
        endereco = endereco - 2;
      }
      if (celula == 3) {
        celula = celula - 3;
        endereco = endereco - 3;
      }

      for (let i = 0; i < this.tamanhoBloco; i++) {
        this.memCache[quadro].celulas[celula + i] = this.memPrincipal[
          endereco + i
        ].data;
      }
    },
    read() {
      this.NumerodeAcessos++;
      this.NumerodeLeituras++;

      const bitsTotal = Math.log(this.memPrincipalMax * 4) / Math.log(2);
      const bitsCelula = bitsTotal - Math.log(this.tamanhoBloco) / Math.log(2);
      const bitsQuadro = bitsCelula - Math.log(this.memCacheMax) / Math.log(2);

      const endereco = parseInt(this.enderecoBusca, 2);
      const rotulo = parseInt(this.enderecoBusca.substring(0, bitsQuadro), 2);
      const quadro = parseInt(
        this.enderecoBusca.substring(bitsQuadro, bitsCelula),
        2
      );
      const celula = parseInt(
        this.enderecoBusca.substring(bitsCelula, bitsTotal),
        2
      );

      if (this.memCache[quadro].rotulo != rotulo.toString(2)) {
        this.writeOnCache(endereco, rotulo, quadro, celula);
        this.NumerodeFaltas++;
        this.NumerodeFaltasnaLeitura++;
      } else {
        this.NumerodeAcertos++;
        this.NumerodeAcertosnaLeitura++;
      }

      this.dadoBusca = this.memCache[quadro].celulas[celula];
    },
    write() {
      this.NumerodeAcessos++;
      this.NumerodeEscritas++;

      const bitsTotal = Math.log(this.memPrincipalMax * 4) / Math.log(2);
      const bitsCelula = bitsTotal - Math.log(this.tamanhoBloco) / Math.log(2);
      const bitsQuadro = bitsCelula - Math.log(this.memCacheMax) / Math.log(2);

      const endereco = parseInt(this.enderecoBusca, 2);
      const rotulo = parseInt(this.enderecoBusca.substring(0, bitsQuadro), 2);
      const quadro = parseInt(
        this.enderecoBusca.substring(bitsQuadro, bitsCelula),
        2
      );
      const celula = parseInt(
        this.enderecoBusca.substring(bitsCelula, bitsTotal),
        2
      );

      if (this.memCache[quadro].rotulo != rotulo.toString(2)) {
        this.writeOnCache(endereco, rotulo, quadro, celula);
        this.NumerodeFaltas++;
        this.NumerodeFaltasnaEscrita++;
      } else {
        this.NumerodeAcertos++;
        this.NumerodeAcertosnaEscrita++;
      }

      this.memCache[quadro].celulas[celula] = this.dadoBusca;
      this.memPrincipal[endereco].data = this.dadoBusca;
    },
    refactorSizes() {
      // this.tamanhoBloco = this.$refs["tamanhoBloco"].val();
      // (this.memPrincipalMax = this.$refs["tamanhoBloco"].val()),
      // (this.memCacheMax = this.$refs["tamanhoBloco"].val()),
      // (this.memPrincipal = []),
      // (this.memCache = []),
      // (this.enderecoBusca = ""),
      // (this.dadoBusca = ""),
      // (this.NumerodeAcessos = 0),
      // (this.NumerodeAcertos = 0),
      // (this.NumerodeFaltas = 0),
      // (this.NumerodeLeituras = 0),
      // (this.NumerodeEscritas = 0),
      // (this.NumerodeAcertosnaLeitura = 0),
      // (this.NumerodeAcertosnaEscrita = 0),
      // (this.NumerodeFaltasnaLeitura = 0),
      // (this.NumerodeFaltasnaEscrita = 0);
      this.popularMemoriaPrincipal();
      this.popularMemoriaCache();
    }
  },
  created: function() {
    this.popularMemoriaPrincipal();
    this.popularMemoriaCache();
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
