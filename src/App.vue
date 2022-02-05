<template>
  <div id="app">
    <header-app @searchResult="searchResult" />
    <main-container
      :filteredFilmsList="filteredFilmsList"
      :filteredSeriesList="filteredSeriesList"
      :msgNoResult="msgNoResult"
      :nResults="nResults"
    />
    <popular-trend
      :filteredPopularFilms="filteredPopularFilms"
      :filteredPopularSeries="filteredPopularSeries"
    />
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
      nResults:0,
    }
  },
  methods: {
    async callApi(queries, type, myRequest) {
      if (queries.query !== '') {
        let params = {
          api_key: this.apiKey,
          language: 'it',
        }
        params = { ...params, ...queries }
        const result = await axios
          .get(`${myRequest}${type}`, { params })
          .then((response) => {
            if (response.data.total_results === 0) {
              this.msgNoResult =
                'Nessun Titolo soddisfa i criteri della ricerca'
              return []
            } else {
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
      this.filteredFilmsList = await this.callApi(
        queries,
        'movie',
        'https://api.themoviedb.org/3/search/',
      )
      this.nResults += this.filteredFilmsList.length
    },
    async searchSeriesByTitle(queries) {
      this.filteredSeriesList = await this.callApi(
        queries,
        'tv',
        'https://api.themoviedb.org/3/search/',
      )
      this.nResults += this.filteredSeriesList.length
    },
    async searchResult(queries) {
      this.nResults=0
      await this.searchFilmsByTitle(queries)
      await this.searchSeriesByTitle(queries)
      console.log(this.nResults + '*300 =' + this.nResults*300)
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
    this.searchPopularSeries(popular, 'https://api.themoviedb.org/3/tv/popular')
  },
  computed: {
    nResultsComputed(){
      return this.nResults
    }
  },
}
</script>

<style lang="scss">
@import './style/main.scss';
</style>
