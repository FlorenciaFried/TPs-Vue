<template>
  <footer class="footer">
    <p class="text">Tecnología y Gestión Web - 2020</p>
  </footer>
</template>

<script>
export default {
  name: 'Footer',
}
</script>

<style scoped>
.footer {
  box-shadow: 0 0 20px 0 rgba(0, 0, 0, 0.5);
  width: 100%;
  height: 10vh;
  bottom: 0 !important;
  position: absolute;
  background-color: #C2AE7A;
}
.text {
  float: right;
  margin-right: 20px;
  font-size: x-large;
  font-weight: bold;
  color: white;
}
</style>