<template>
  <div id="app" class="container mt-5 mb-5" style="width:500px;">
    <FormValidation></FormValidation>
  </div>
</template>

<script>
import FormValidation from './components/FormValidation.vue'

export default {
  name: 'app',
  components: {
    FormValidation
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
