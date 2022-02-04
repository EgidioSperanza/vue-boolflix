<template>
  <div id="app">
    <header-app @searchByTitle="searchByTitle" />
    <main-container
      :filteredFilmsList="filteredFilmsList"
      :filteredSeriesList="filteredSeriesList"
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
      apiKey: 'e0151c8f32093eec292358373e03c7c1',
      searchInApi: 'https://api.themoviedb.org/3/search/',
      filteredFilmsList: [],
      filteredSeriesList: [],
      msgNoResult: '',
    }
  },
  methods: {
    async searchFilmsByTitle(title, familyFilter) {
      const params = {
        api_key: this.apiKey,
        query: title,
        include_adult: !familyFilter,
        language: 'it',
      }
      const filmsApi = `${this.searchInApi}movie`
      const requestFilms = axios.get(`${filmsApi}`, { params })

      await requestFilms.then((response) => {
        if (response.data.total_results === 0) {
          this.msgNoResult = 'Nessun Titolo soddisfa i criteri di ricerca'
        }
        this.filteredFilmsList = response.data.results
      })
    },
    async searchSeriesByTitle(title, familyFilter) {
      const params = {
        api_key: this.apiKey,
        query: title,
        include_adult: familyFilter,
        language: 'it',
      }
      const seriesApi = `${this.searchInApi}tv`
      const requestSeries = axios.get(`${seriesApi}`, { params })

      await requestSeries.then((response) => {
        if (response.data.total_results === 0) {
          this.msgNoResult = 'Nessun Titolo soddisfa i criteri di ricerca'
        }
        this.filteredSeriesList = response.data.results
      })
    },
    async searchByTitle(title, familyFilter) {
      if (title === '') {
        this.msgNoResult = 'Digita Qualcosa Prima'
      } else {
        await this.searchFilmsByTitle(title, familyFilter)
        await this.searchSeriesByTitle(title, familyFilter)
      }
    },
  },
  mounted() {
    
  },
}
</script>

<style lang="scss">
@import './style/main.scss';
</style>
