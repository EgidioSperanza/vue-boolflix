<template>
  <div id="app">
    <header-app @searchResult="searchResult" />
    <main-container
      :filteredFilmsList="filteredFilmsList"
      :filteredSeriesList="filteredSeriesList"
      :msgNoResult="msgNoResult"
    />
    <popular-trend :filteredPopularFilms="filteredPopularFilms" />
  </div>
</template>

<script>
import axios from 'axios'
import HeaderApp from './components/HeaderApp.vue'
import MainContainer from './components/MainContainer.vue'
import PopularTrend from './components/PopularTrend.vue'

export default {
  name: 'App',
  components: {
    HeaderApp,
    MainContainer,
    PopularTrend,
  },
  data() {
    return {
      apiKey: 'e0151c8f32093eec292358373e03c7c1',
      filteredFilmsList: [],
      filteredSeriesList: [],
      filteredPopularFilms: [],
      msgNoResult: '',
    }
  },
  methods: {
    async callApi(queries, type, action) {
      if (queries.query !== '') {
        let params = {
          api_key: this.apiKey,
          language: 'it',
        }
        this.queries = queries
        params = { ...params, ...queries }
        console.log(params)
        action="https://api.themoviedb.org/3/search/"
        const result = await axios
          .get(`${action}${type}`, { params })
          .then((response) => {
            if (response.data.total_results === 0) {
              this.msgNoResult =
                'Nessun Titolo soddisfa i criteri della ricerca'
              console.log('Risponde', 'vuoto')
              return []
            } else {
              console.log('Risponde', response.data.result)
              return response.data.results
            }
          })
        return result
      } else {
        this.msgNoResult = 'Digita Qualcosa per effettuare una ricerca'
      }
      return []
    },
    async searchFilmsByTitle(queries) {
      this.filteredFilmsList = await this.callApi(queries, 'movie')
    },
    async searchSeriesByTitle(queries) {
      this.filteredSeriesList = await this.callApi(queries, 'tv')
    },
    searchResult(queries) {
      this.searchFilmsByTitle(queries)
      this.searchSeriesByTitle(queries)
    },
    showPopularFilms(){
      let popularMovies = {
        query: 'get-popular-movies',
        include_adult: false,
      }
      this.filteredPopularFilms =  this.callApi(popularMovies, '', 'https://api.themoviedb.org/3/get-popular-movies')
    },
  },
}
</script>

<style lang="scss">
@import './style/main.scss';
</style>
