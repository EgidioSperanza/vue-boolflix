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
    searchByTitle(title, familyFilter) {
      axios
        .get(
          `https://api.themoviedb.org/3/search/movie?api_key=e0151c8f32093eec292358373e03c7c1&language=it&adult=false&query=${title}&include_adult=${!familyFilter}`,
          
        )
        .then((response) => {

          this.filteredFilmsList = response.data.results
        })
    },
  },
}
</script>

<style lang="scss">
@import './style/main.scss';
</style>
