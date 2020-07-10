<template>
  <div class="container">
    <div class="section">
      <h1 class="titulo">La Plata, Buenos Aires</h1>
      <p>Temperatura: {{ (parseInt(infolp.temp) - 273.15).toFixed(2) }}°C</p>
      <p>
        Sensación térmica:
        {{ (parseInt(infolp.feels_like) - 273.15).toFixed(2) }}°C
      </p>
      <p>
        Temperatura máxima:
        {{ (parseInt(infolp.temp_max) - 273.15).toFixed(2) }}°C
      </p>
      <p>
        Temperatura mínima:
        {{ (parseInt(infolp.temp_min) - 273.15).toFixed(2) }}°C
      </p>
      <p>Humedad: {{ infolp.humidity }}%</p>
    </div>

    <div class="section">
      <h1 class="titulo">Ushuaia, Tierra del Fuego</h1>
      <p>Temperatura: {{ (parseInt(infou.temp) - 273.15).toFixed(2) }}°C</p>
      <p>
        Sensación térmica:
        {{ (parseInt(infou.feels_like) - 273.15).toFixed(2) }}°C
      </p>
      <p>
        Temperatura máxima:
        {{ (parseInt(infou.temp_max) - 273.15).toFixed(2) }}°C
      </p>
      <p>
        Temperatura mínima:
        {{ (parseInt(infou.temp_min) - 273.15).toFixed(2) }}°C
      </p>
      <p>Humedad: {{ infou.humidity }}%</p>
    </div>

    <div class="section">
      <h1 class="titulo">La Quiaca, Jujuy</h1>
      <p>Temperatura: {{ (parseInt(infoq.temp) - 273.15).toFixed(2) }}°C</p>
      <p>
        Sensación térmica:
        {{ (parseInt(infoq.feels_like) - 273.15).toFixed(2) }}°C
      </p>
      <p>
        Temperatura máxima:
        {{ (parseInt(infoq.temp_max) - 273.15).toFixed(2) }}°C
      </p>
      <p>
        Temperatura mínima:
        {{ (parseInt(infoq.temp_min) - 273.15).toFixed(2) }}°C
      </p>
      <p>Humedad: {{ infoq.humidity }}%</p>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'Clima',

  data: function () {
    return {
      infolp: '',
      errorlp: '',
      infou: '',
      erroru: '',
      infoq: '',
      errorq: '',
    }
  },

  methods: {
    callToApi: async function (ciudad) {
      await axios
        .get("http://api.openweathermap.org/data/2.5/weather?q=" + ciudad + ",Argentina&appid=9b147eb1e617afe522a5c074780d5379")
        .then((response) => {
          if (ciudad === 'La Plata') {
            this.infolp = response.data.main
          }
          if (ciudad === 'Ushuaia') {
            this.infou = response.data.main
          }
          if (ciudad === 'La Quiaca') {
            this.infoq = response.data.main
          }
        })
        .catch((error) => {
          if (ciudad === 'La Plata') {
            console.log(error)
            this.errorlp = error;
          }
          if (ciudad === 'Ushuaia') {
            console.log(error)
            this.infou = error;
          }
          if (ciudad === 'La Quiaca') {
            console.log(error)
            this.infoq = error;
          }
        })
    },
  },

  mounted() {
    this.callToApi('La Plata')
    this.callToApi('Ushuaia')
    this.callToApi('La Quiaca')
  },
}
</script>

<style scoped>
.container {
  height: 100%;
  width: 100%;
}

.section {
  height: 100%;
  width: 33%;
  display: inline-block;
  text-align: center;
  color: rgb(95, 83, 92);
}
</style>