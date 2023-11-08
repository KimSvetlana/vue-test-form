<template>
  <div id="app" class="app-style" >
    <RegistrForm> </RegistrForm>
  </div>
</template>

<script>
import RegistrForm from './components/RegistrForm.vue';

export default {
  name: 'App',
  components: {
    RegistrForm
  }
}
</script>

<style>
  #app {
    font-family: 'Montserrat', sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
  }
  .app-style {
    display: flex;
    align-items: center;
    min-height: 100vh;
    background-color: #f3f3f7;
  }
</style>
