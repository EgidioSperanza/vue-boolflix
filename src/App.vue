<template>
  <div id="app">
    <header-app />
    <main-container />
  </div>
</template>

<script>
import HeaderApp from './components/HeaderApp.vue'
import MainContainer from './components/MainContainer.vue'

export default {
  name: 'App',
  components: {
    HeaderApp,
    MainContainer,
  },
}
</script>

<style lang="scss">
@import './style/main.scss';
</style>
