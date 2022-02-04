<template>
  <div id="app">
    <header-app @searchResult="searchResult" />
    <main-container
      :filteredFilmsList="filteredFilmsList"
      :filteredSeriesList="filteredSeriesList"
      :msgNoResult="msgNoResult"
    />
    <popular-trend :filteredPopularFilms="filteredPopularFilms" :filteredPopularSeries="filteredPopularSeries"/>
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
      filteredPopularSeries: [],
      msgNoResult: '',
    }
  },
  methods: {
    async callApi(queries, type, myRequest) {
      if (queries.query !== '') {
        let params = {
          api_key: this.apiKey,
          language: 'it',
        }
        this.queries = queries
        params = { ...params, ...queries }
        console.log(params)
        const result = await axios
          .get(`${myRequest}${type}`, { params })
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
      this.filteredFilmsList = await this.callApi(queries, 'movie','https://api.themoviedb.org/3/search/')
    },
    async searchSeriesByTitle(queries) {
      this.filteredSeriesList = await this.callApi(queries, 'tv','https://api.themoviedb.org/3/search/')
    },
    searchResult(queries) {
      this.searchFilmsByTitle(queries)
      this.searchSeriesByTitle(queries)
    },
    async searchPopularFilms(queries, myRequest) {
      this.filteredPopularFilms = await this.callApi(queries, '', myRequest)
    },
    async searchPopularSeries(queries, myRequest) {
      this.filteredPopularSeries = await this.callApi(queries, '', myRequest)
    },
  },
  mounted() {
    let popular = {
      api_key: this.apiKey,
      include_adult: false,
      language: 'it',
    }
    this.searchPopularFilms(
      popular,
      'https://api.themoviedb.org/3/movie/popular',
    )
    this.searchPopularSeries(
      popular,
      'https://api.themoviedb.org/3/tv/popular',
    )
  },
}
</script>

<style lang="scss">
@import './style/main.scss';
</style>
