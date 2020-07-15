<template>
  <div class="container">
    <h1 v-if="check === false">
      Debe presionar el boton "Generar información"
    </h1>

    <div class="chartContainer" v-if="check === true">
      <GChart type="ColumnChart" :data="chartData" :options="chartOptions" />
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import { GChart } from 'vue-google-charts'

export default {
  name: 'Chart1',

  components: {
    GChart
  },

  props: {
    check: {
      type: Boolean,
      required: true,
    },
  },

  data() {
    return {
      token: '',
      data: null,
      datos: [["Ciudad", "Temperatura mínima", "Temperatura máxima"]],
      chartData: [],
      chartOptions: {
        chart: {
          title: "Company Performance",
          subtitle: "Sales, Expenses, and Profit: 2014-2017"
        },
        width: 900,
      },
    }
  },

  methods: {
    callToStrapi: async function () {
      await axios.get("http://localhost:1337/climas", {
        headers: {
          Authorization: `Bearer ${this.token}`
        },
      }).then(response => {
        this.data = response.data.slice(response.data.length - 3)
        console.log(this.data)
      })
    },

    generarTabla: function () {
      this.datos.push(["La Plata", this.data[0].temp_min, this.data[0].temp_max])
      this.datos.push(["Ushuaia", this.data[1].temp_min, this.data[1].temp_max])
      this.datos.push(["La Quiaca", this.data[2].temp_min, this.data[2].temp_max])

      this.chartData = this.datos;

    },
  },

  async mounted() {
    await axios.post("http://localhost:1337/auth/local", {
      identifier: 'api-user@example.com',
      password: 'fg*QD-Lg-.!d4*E'
    }).then(response => {
      this.token = response.data.jwt;

      this.callToStrapi()
        .then(() => {
          this.generarTabla();
        });
    })
  },
}
</script>

<style scoped>
.container {
  text-align: center;
}

.container h1 {
  color: #c28b7a;
}

.chartContainer {
  display: flex;
  justify-content: center;
  margin-top: 80px;
}
</style>