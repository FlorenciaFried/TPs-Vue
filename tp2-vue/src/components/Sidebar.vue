<template>
  <div class="container">
    <aside class="sidebar">
      <button class="button" v-on:click="type = 'generar'">
        GENERAR INFORMACION
      </button>
      <button class="button" v-on:click="type = 'visualizar'">
        VISUALIZAR INFORMACION
      </button>
    </aside>

    <section class="data-container">
      <div class="presentacion" v-if="type === ''">
        <h1>Bienvenidos a mi APP</h1>
        <h2>Las tecnologías usadas son:</h2>
        <ul>
          <li>Vue.js</li>
          <li>Axios</li>
          <li>Strapi</li>
          <li>Google Charts</li>
        </ul>
      </div>

      <Clima v-if="type === 'generar'"></Clima>
    </section>
  </div>
</template>

<script>
import Clima from './Clima';

export default {
  name: 'Sidebar',

  components: {
    Clima,
  },

  data: function () {
    return {
      type: ''
    }
  },

  methods: {
  }
}
</script>

<style scoped>
.container {
  width: 100%;
  height: 70vh;
}

.sidebar {
  width: 20%;
  height: 100%;
  color: #fff;
  float: left;
  padding-top: 30px;
}

.button {
  background-color: #c2ae7a;
  color: rgb(119, 100, 100);
  padding: 20px;
  margin: 10px;
  margin-top: 20px;
  width: 230px;
  border: 1px solid black;
  border-radius: 3px;
  font-weight: bold;
  transform: scale(1);
  transition: 0.2s;
}

.button:hover {
  cursor: pointer;
  color: rgb(255, 255, 255);
  border: 1px solid white;
  transform: scale(1.1);
  transition: 0.2s;
}

.data-container {
  width: 80%;
  height: 70vh;
  float: left;
}

.presentacion {
  margin-left: 365px;
}

.presentacion h1 {
  font-weight: bold;
}

.presentacion h1,
h2 {
  color: #c28b7a;
}

.presentacion ul {
  list-style: none;
}

.presentacion ul li {
  margin-top: 10px;
}
</style>