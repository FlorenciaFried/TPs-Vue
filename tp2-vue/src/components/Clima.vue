<template>
  <div>
    {{ callToApi('La Plata') }}
    {{ callToApi('Mar del Plata') }}
    <h1>{{ info }}</h1>
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
      infomdq: '',
      errormdq: '',
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
          if (ciudad === 'Mar del Plata') {
            this.infomdq = response.data.main

          }
        })
        .catch((error) => {
          if (ciudad === 'La Plata') {
            console.log(error)
            this.errorlp = error;
          }
          if (ciudad === 'Mar del Plata') {
            console.log(error)
            this.errormdq = error;
          }
        })
    },
  },
}
</script>

<style scoped>
</style>