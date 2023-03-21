<template>
  <v-app>
    <v-main>
      <v-container>
        <v-row>
          <v-col>
            <h1>Seleccione un año</h1>
            <date-picker
              v-model:value="year"
              @change="searchUF"
              type="year"
              placeholder="Select year"
            >
            </date-picker>
          </v-col>
        </v-row>
        El valor de la UF se calcula mediante el IPC del mes anterior, desde el
        día 10 actual hasta el día 9 del mes siguiente.

        <h1 style="color: gray" v-if="values.length == 0">Sin datos</h1>
        <v-progress-circular v-if="show" indeterminate></v-progress-circular>
        <v-container v-if="values.length > 0">
          <h1>Máximos y minimos del {{ year.getFullYear() }}</h1>
          <v-row dense>
            <!-- Maximo y minimo -->
            <v-col cols="2">
              <Value :title="'Valor máximo'" :value="maximo" class="roboto_mono"></Value>
            </v-col>
            <v-col cols="2">
              <Value :title="'Valor mínimo'" :value="minimo" class="roboto_mono"></Value>
            </v-col>
          </v-row>

          <v-row>
            <v-col>
              <h1>Variaciones mes a mes</h1>

              <Ipcs :ipcs="ipcs" class="roboto_mono"></Ipcs>
            </v-col>

            <v-col>
              <h1>Alzas de la UF, ordenadas de menor a mayor</h1>

              <Ipcs :ipcs="IPCsHigh" class="roboto_mono"></Ipcs>

              <h2 style="color: gray" v-if="IPCsHigh.length == 0">Sin datos</h2>
            </v-col>

            <v-col>
              <h1>Bajas de la UF, ordenadas de mayor a menor</h1>

              <Ipcs :ipcs="IPCsLow" class="roboto_mono"></Ipcs>
              <h2 style="color: gray" v-if="IPCsLow.length == 0">Sin datos</h2>
            </v-col>

            <v-col>
              <h1>Periodos de estabilidad de la UF</h1>

              <Ipcs :ipcs="IPCsQuiet" class="roboto_mono"></Ipcs>
              <h2 style="color: gray" v-if="IPCsQuiet.length == 0">
                Sin datos
              </h2>
            </v-col>
          </v-row>
        </v-container>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
import DatePicker from "vue-datepicker-next";
import "vue-datepicker-next/index.css";
import Ipcs from "../components/Ipcs.vue";
import Value from "../components/Value.vue";
import axios from "axios";
export default {
  components: { DatePicker, Ipcs, Value },
  // ...
  data() {
    return {
      year: new Date(),
      values: [],
      maximo: 0,
      minimo: 0,
      ipcs: [],
      IPCsHigh: [],
      IPCsLow: [],
      IPCsQuiet: [],
      show : false,
      url: "https://mindicador.cl/api/",
    };
  },
  created() {
    this.searchUF();
  },
  methods: {
    searchUF() {
      this.values = [];
      axios
        .get(this.url + "uf/" + this.year.getFullYear())
        .then((response) => {
          this.values = response.data.serie;
          this.maximo = Math.max.apply(
            Math,
            this.values.map(function (o) {
              return o.valor;
            })
          );
          this.maximo = this.values.find(
            (element) => element.valor == this.maximo
          );
          this.minimo = Math.min.apply(
            Math,
            this.values.map(function (o) {
              return o.valor;
            })
          );
          this.minimo = this.values.find(
            (element) => element.valor == this.minimo
          );
        })
        .catch((error) => {
          console.log(error);
          this.values = null;
        });
      this.ipc();
    },
    async ipc() {
      this.ipcs = [];
      this.IPCsHigh = [];
      this.IPCsLow = [];
      this.IPCsQuiet = [];
      this.show = true;
      var respuesta = await axios.get(
        this.url + "ipc/" + this.year.getFullYear()
      );
      this.show = false;
      respuesta = respuesta.data.serie;
      var passedDecember = await axios.get(
        this.url + "ipc/" + (this.year.getFullYear() - 1)
      );
      passedDecember = passedDecember.data.serie[0];
      respuesta.reverse();
      respuesta.unshift(passedDecember);
      if (respuesta.length > 12) {
        respuesta.pop();
      }

      var meses = [
        "Enero",
        "Febrero",
        "Marzo",
        "Abril",
        "Mayo",
        "Junio",
        "Julio",
        "Agosto",
        "Septiembre",
        "Octubre",
        "Noviembre",
        "Diciembre",
      ];
      respuesta.forEach((element, i) => {
        var mes = meses[i];
        var variacion = element.valor;
        this.ipcs.push({ mes: mes, IPC: variacion });
        if (element.valor > 0) {
          this.IPCsHigh.push({ mes: mes, IPC: variacion });
        }
        if (element.valor < 0) {
          this.IPCsLow.push({ mes: mes, IPC: variacion });
        }
        if (element.valor == 0) {
          this.IPCsQuiet.push({ mes: mes, IPC: variacion });
        }

        console.log(mes, variacion);
      });
      this.IPCsHigh.sort(function (a, b) {
        return a.IPC - b.IPC;
      });
      this.IPCsLow.sort(function (a, b) {
        return a.IPC - b.IPC;
      });
      //Order by alphabetic mes
      this.IPCsQuiet.sort(function (a, b) {
        if (a.mes < b.mes) {
          return -1;
        }
        if (a.mes > b.mes) {
          return 1;
        }
        return 0;
      });
    },
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Roboto+Mono:wght@100;400&display=swap");

.roboto_mono {
  font-family: "Roboto Mono", monospace;
}
</style>
