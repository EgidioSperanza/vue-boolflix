<template>
  <div id="app">
    <header-app @searchByTitle="searchByTitle" />
    <main-container :filteredFilmsList="filteredFilmsList" />
  </div>
</template>

<script>
import axios from 'axios'
import HeaderApp from './components/HeaderApp.vue'
import MainContainer from './components/MainContainer.vue'

export default {
  name: 'App',
  components: {
    HeaderApp,
    MainContainer,
  },
  data() {
    return {
      filteredFilmsList: [],
    }
  },
  methods: {
    searchByTitle(title) {
      axios
        .get(
          `https://api.themoviedb.org/3/search/movie?api_key=e0151c8f32093eec292358373e03c7c1&language=it&query=${title}`,
        )
        .then((response) => {
          this.filteredFilmsList = response.data.results
        })
        console.log(this.filteredFilmsList)
        return this.filteredFilmsList
    },
  },
}
</script>

<style lang="scss">
@import './style/main.scss';
</style>
