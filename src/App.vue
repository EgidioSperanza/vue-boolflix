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
      :nPopularFilmsResults="nPopularFilmsResults"
      :nPopularSeriesResults="nPopularSeriesResults"
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
      nResults: 0,
      nPopularFilmsResults: 0,
      nPopularSeriesResults: 0,
    }
  },
  methods: {
    // METHOD TO CALL THE API {{REQUEST}}
    // {{TYPE}} (movie tv and other)
    // ADDING THE AUTHENTICATION KEY,
    // THE LANGUAGE AND ANY OTHER {{QUERIES}}
    // (STRING TO SEARCH, FAMILY FILTER ETC ...)
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
    // FILL THE DATA ARRAY FILMS VIA THE GENERIC API CALL
    // ADDS THE NUMBER OF ITEMS TO A DATA
    async searchFilmsByTitle(queries) {
      this.filteredFilmsList = await this.callApi(
        queries,
        'movie',
        'https://api.themoviedb.org/3/search/',
      )
      this.nResults += this.filteredFilmsList.length
    },
    // FILL THE DATA ARRAY SERIES VIA THE GENERIC API CALL
    // ADDS THE NUMBER OF ITEMS TO A DATA
    async searchSeriesByTitle(queries) {
      this.filteredSeriesList = await this.callApi(
        queries,
        'tv',
        'https://api.themoviedb.org/3/search/',
      )
      this.nResults += this.filteredSeriesList.length
    },
    // THROUGH A CHILD COMPONENT WE PASS A SERIES OF QUERIES
    //  TO OBTAIN FROM A STRING A LIST OF FILMS AND TV SERIES
    // NB ONLY THE FIRST PAGE OF THE POSSIBLE RESULTS
    // TODO: ADD TO THE CHILD COMPONENT SHOWING THE LISTS A LAST ELEMENT
    //  THAT FROM TIME TO TIME CALLS THE API THROUGH THE PARENT TO ADD THE
    //  NEXT PAGES OF POSSIBLE RESULTS
    async searchResult(queries) {
      this.nResults = 0
      await this.searchFilmsByTitle(queries)
      await this.searchSeriesByTitle(queries)
    },
    // API CALL FOR TREND FILMS LIST
    async searchPopularFilms(queries, myRequest) {
      this.filteredPopularFilms = await this.callApi(queries, '', myRequest)
      this.nPopularFilmsResults += this.filteredPopularFilms.length
    },
    // API CALL FOR TREND FILMS LIST
    async searchPopularSeries(queries, myRequest) {
      this.filteredPopularSeries = await this.callApi(queries, '', myRequest)
      this.nPopularSeriesResults += this.filteredPopularSeries.length
    },
  },
  mounted() {
    // AUTOMATIC-DEFAULT PAGE FILLING WITH TWO LISTS REGARDING TREND FILMS AND TV SERIES
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
    // WE KEEP THE NUMBERS OF THE LIST ELEMENTS UPDATED FOR
    // A VIEW AND SCROLL RELATED TO THE VIEWPORT 
    nResultsComputed() {
      return this.nResults
    },
    nPopularFilmsResultsComputed() {
      return this.nPopularFilmsResults
    },
    nPopularSeriesResultsComputed() {
      return this.nPopularSeriesResults
    },
  },
}
</script>

<style lang="scss">
@import './style/main.scss';
</style>
