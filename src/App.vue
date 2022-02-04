<template>
  <div id="app">
    <header-app @searchByTitle="searchByTitle" />
    <main-container
      :filteredSearchList="filteredSearchList"
      :msgNoResult="msgNoResult"
    />
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
      filteredSearchList: [],
      msgNoResult: '',
    }
  },
  methods: {
    searchByTitle(title, familyFilter) {
      this.msgNoResult = ''
      if (title === '') {
        this.filteredSearchList=[]
        this.msgNoResult = 'Devi Digitare qualcosa per effettuare una ricerca'
      } else {
        let filteredFilmsList = []
        let filteredSeriesList = []
        axios
        let films = `https://api.themoviedb.org/3/search/movie?api_key=e0151c8f32093eec292358373e03c7c1&language=it&adult=false&query=${title}&include_adult=${!familyFilter}`
        let series = `https://api.themoviedb.org/3/search/tv?api_key=e0151c8f32093eec292358373e03c7c1&language=it&adult=false&query=${title}&include_adult=${!familyFilter}`
        const requestFilms = axios.get(films)
        const requestSeries = axios.get(series)
        this.filteredSearchList = []
        requestFilms.then((response) => {
          if (response.data.total_results === 0) {
            this.msgNoResult = 'Nessun Titolo soddisfa i criteri di ricerca'
          }
          filteredFilmsList = response.data.results
          this.filteredSearchList = filteredFilmsList
        })
        requestSeries.then((response) => {
          if (response.data.total_results === 0) {
            this.msgNoResult = 'Nessun Titolo soddisfa i criteri di ricerca'
          }
          filteredSeriesList = response.data.results
          filteredSeriesList.forEach((element) => {
            this.filteredSearchList.push(element)
          })
        })
      }
    },
  },
}
</script>

<style lang="scss">
@import './style/main.scss';
</style>
