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
      infou: '',
      infoq: '',
      token: '',
    }
  },

  methods: {
    callToApi: async function (ciudad) {
      await axios
        .get("http://api.openweathermap.org/data/2.5/weather?q=" + ciudad + ",Argentina&appid=9b147eb1e617afe522a5c074780d5379")
        .then((response) => {
          if (ciudad === 'La Plata') {
            this.infolp = response.data.main
            this.createClima('La Plata')
          }
          if (ciudad === 'Ushuaia') {
            this.infou = response.data.main
            this.createClima('Ushuaia')
          }
          if (ciudad === 'La Quiaca') {
            this.infoq = response.data.main
            this.createClima('La Quiaca')
          }
        })
    },

    createClima: function (ciudad) {
      if (ciudad === 'La Plata') {
        axios.post('http://localhost:1337/climas', {
          headers: {
            Authorization: `Bearer ${this.token}`
          },
          temp: (parseInt(this.infolp.temp) - 273.15).toFixed(2),
          temp_max: (parseInt(this.infolp.temp_max) - 273.15).toFixed(2),
          temp_min: (parseInt(this.infolp.temp_min) - 273.15).toFixed(2),
          sensacion_termica: (parseInt(this.infolp.feels_like) - 273.15).toFixed(2),
          humedad: parseInt(this.infolp.humidity),
          ciudad: 'La Plata',
        })
      }

      if (ciudad === 'Ushuaia') {
        axios.post('http://localhost:1337/climas', {
          headers: {
            Authorization: `Bearer ${this.token}`
          },
          temp: (parseInt(this.infou.temp) - 273.15).toFixed(2),
          temp_max: (parseInt(this.infou.temp_max) - 273.15).toFixed(2),
          temp_min: (parseInt(this.infou.temp_min) - 273.15).toFixed(2),
          sensacion_termica: (parseInt(this.infou.feels_like) - 273.15).toFixed(2),
          humedad: parseInt(this.infou.humidity),
          ciudad: 'Ushuaia',
        })
      }

      if (ciudad === 'La Quiaca') {
        axios.post('http://localhost:1337/climas', {
          headers: {
            Authorization: `Bearer ${this.token}`
          },
          temp: (parseInt(this.infoq.temp) - 273.15).toFixed(2),
          temp_max: (parseInt(this.infoq.temp_max) - 273.15).toFixed(2),
          temp_min: (parseInt(this.infoq.temp_min) - 273.15).toFixed(2),
          sensacion_termica: (parseInt(this.infoq.feels_like) - 273.15).toFixed(2),
          humedad: parseInt(this.infoq.humidity),
          ciudad: 'La Quiaca',
        })
      }
    }
  },

  mounted() {
    axios.post("http://localhost:1337/auth/local", {
      identifier: 'florencia.d.fried@gmail.com',
      password: 'fg*QD-Lg-.!d4*E'
    }).then(response => {
      this.token = response.data.jwt;

      this.callToApi('La Plata')
      this.callToApi('Ushuaia')
      this.callToApi('La Quiaca')
    })
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