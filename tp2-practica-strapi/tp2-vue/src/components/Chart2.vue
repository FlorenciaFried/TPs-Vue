<template>
  <div class="container">
    <h1 v-if="check === false">
      Debe presionar el boton "Generar información"
    </h1>

    <div class="chartContainer" v-if="check === true">
      <GChart
        :settings="{ packages: ['bar'] }"
        :data="chartData"
        :options="chartOptions"
        :createChart="(el, google) => new google.charts.Bar(el)"
        @ready="onChartReady"
      />
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
      datos: [["Ciudad", "Sensacion termica", "Temperatura", "Humedad"]],
      chartsLib: null,
      chartData: [],
    }
  },

  computed: {
    chartOptions() {
      if (!this.chartsLib) return null
      return this.chartsLib.charts.Bar.convertOptions({
        chart: {
          title: 'Comparacion entre ciudades',
          subtitle: 'Sensacion termica, Temperatura, y Humedad'
        },
        bars: 'horizontal', 
        hAxis: { format: 'decimal' },
        height: 400,
        colors: ['#1b9e77', '#d95f02', '#7570b3']
      })
    },
  },

  methods: {
    callToStrapi: async function () {
      await axios.get("http://localhost:1337/climas", {
        headers: {
          Authorization: `Bearer ${this.token}`
        },
      }).then(response => {
        this.data = response.data.slice(response.data.length - 3)
      })
    },

    generarTabla: function () {
      this.datos.push(['La Plata', this.data[0].sensacion_termica, this.data[0].temp, this.data[0].humedad])
      this.datos.push(['Ushuaia', this.data[1].sensacion_termica, this.data[1].temp, this.data[1].humedad])
      this.datos.push(['La Quiaca', this.data[2].sensacion_termica, this.data[2].temp, this.data[2].humedad])
      this.chartData = this.datos;
    },

    onChartReady(chart, google) {
      this.chartsLib = google
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
}
</style>