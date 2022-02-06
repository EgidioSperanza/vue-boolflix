<template>
  <div id="app">
    <header-app @searchResult="searchResult" />
    <main-container
      :filteredFilmsList="filteredFilmsList"
      :filteredSeriesList="filteredSeriesList"
      :msgNoResult="msgNoResult"
      :nResults="nResults"
      :sliderPosition="sliderPosition"
      :position="position"
      :viewportWidth="viewportWidth"
      @changePosition="changePosition"
    />
    <popular-trend
      :filteredPopularFilms="filteredPopularFilms"
      :filteredPopularSeries="filteredPopularSeries"
      :nResults="nResults"
      :sliderPosition="sliderPosition"
      :position="position"
      :viewportWidth="viewportWidth"
      @changePosition="changePosition"
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
      nResults: [0, 0, 0],
      sliderPosition: [1, 1, 1],
      position: [0, 0, 0],
      viewportWidth: window.innerWidth - 60, //Slider margin
    }
  },
  // //DEBUG
  watch: {
    viewportWidth(newWidth, oldWidth) {
      console.log(`it changed to ${newWidth} from ${oldWidth}`)
    },
  },

  methods: {
    // METHOD TO CALL THE API {{REQUEST}}
    // {{TYPE}} (movie tv and other)
    // ADDING THE AUTHENTICATION KEY,
    // THE LANGUAGE AND ANY OTHER {{QUERIES}}
    // (STRING TO SEARCH, FAMILY FILTER ETC ...)
    async callApi(queries, type, myRequest) {
      // DO WE KNOW WHAT TO LOOK FOR?
      // IN CASE OF SEARCH WITHOUT STRING, IT WILL NOT MAKE THE CALL
      // UNLESS YOU ENTER FURTHER FIELDS IN {{QUERIES}}
      if (queries.query !== '') {
        let params = {
          api_key: this.apiKey,
          language: 'it',
        }
        params = { ...params, ...queries }
        const result = await axios
          .get(`${myRequest}${type}`, { params })
          .then((response) => {
            // IF THERE ARE NO RESULTS?
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
        // NO STRING TO SEARCH
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
      this.nResults[0] += this.filteredFilmsList.length
    },
    // FILL THE DATA ARRAY SERIES VIA THE GENERIC API CALL
    // ADDS THE NUMBER OF ITEMS TO A DATA
    async searchSeriesByTitle(queries) {
      this.filteredSeriesList = await this.callApi(
        queries,
        'tv',
        'https://api.themoviedb.org/3/search/',
      )
      this.nResults[0] += this.filteredSeriesList.length
    },
    // THROUGH A CHILD COMPONENT WE PASS A SERIES OF QUERIES
    //  TO OBTAIN FROM A STRING A LIST OF FILMS AND TV SERIES
    // NB ONLY THE FIRST PAGE OF THE POSSIBLE RESULTS
    // TODO: ADD TO THE CHILD COMPONENT SHOWING THE LISTS A LAST ELEMENT
    //  THAT FROM TIME TO TIME CALLS THE API THROUGH THE PARENT TO ADD THE
    //  NEXT PAGES OF POSSIBLE RESULTS
    async searchResult(queries) {
      this.nResults[0] = 0
      await this.searchFilmsByTitle(queries)
      await this.searchSeriesByTitle(queries)
      this.sliderPosition[0] = 1
      this.position[0] = this.sliderPosition[0]
    },
    // API CALL FOR TREND FILMS LIST
    async searchPopularFilms(queries, myRequest) {
      this.filteredPopularFilms = await this.callApi(queries, '', myRequest) //TYPE=''
      this.nResults[1] += this.filteredPopularFilms.length
    },
    // API CALL FOR TREND FILMS LIST
    async searchPopularSeries(queries, myRequest) {
      this.filteredPopularSeries = await this.callApi(queries, '', myRequest) //TYPE=''
      this.nResults[2] += this.filteredPopularSeries.length
    },
    // I RECEIVE AND EXECUTE - EMIT
    changePosition(i) {
      this.position[i] = this.sliderPosition[i] * -1
    },
    onResize() {
      this.viewportWidth = window.innerWidth - 60 //Slider margin
      if (this.filteredFilmsList.length > 0 || this.filteredSeriesList > 0) {
        this.sliderPosition[0] = 1
        this.position[0] = this.sliderPosition[0]
      }
    },
  },
  mounted() {
    // AUTOMATIC-DEFAULT PAGE FILLING WITH TWO LISTS REGARDING TREND FILMS AND TV SERIES
    let popular = {
      include_adult: false, //ADDED IN QUERIES FOR OBTAIN A RESULT
    }
    this.searchPopularFilms(
      popular,
      'https://api.themoviedb.org/3/movie/popular',
    )
    this.searchPopularSeries(popular, 'https://api.themoviedb.org/3/tv/popular')
    this.$nextTick(() => {
      window.addEventListener('resize', this.onResize)
    })
  },
  computed: {
    // WE KEEP THE NUMBERS OF THE LIST ELEMENTS UPDATED FOR
    // A VIEW AND SCROLL RELATED TO THE VIEWPORT
    nResultsComputed() {
      return this.nResults
    },
    computedPosition() {
      return this.position
    },
    computedSliderPosition() {
      return this.sliderPosition
    },
    computedViewport() {
      return this.viewportWidth
    },
  },
}
</script>

<style lang="scss">
@import './style/main.scss';
</style>
