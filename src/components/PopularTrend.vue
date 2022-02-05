<template>
  <div>
    <h1>I Films più visti del momento!!!</h1>
    <div class="main-container">
      <div class="slider" :style="{ left: positionFilms + 'px' }">
        <div
          class="result-card"
          v-for="film in filteredPopularFilms"
          :key="film.id"
        >
          <result-card :result="film" />
        </div>
      </div>
    </div>
    <input
      type="range"
      min="1"
      :max="
        nPopularFilmsResults * 300 > viewportWidth
          ? nPopularFilmsResults * 300 - viewportWidth
          : nPopularFilmsResults * 300
      "
      v-model="sliderPositionFilms"
      @input="changePositionFilms"
      class="slider"
      id="myRange"
    />

    <h1>Le Serie Tv più amate del momento!!!</h1>
    <div class="main-container">
      <div class="slider" :style="{ left: positionSeries + 'px' }">
        <div
          class="result-card"
          v-for="serie in filteredPopularSeries"
          :key="serie.id"
        >
          <result-card :result="serie" />
        </div>
      </div>
    </div>
    <input
      type="range"
      min="1"
      :max="
        nPopularSeriesResults * 300 > viewportWidth
          ? nPopularSeriesResults * 300 - viewportWidth
          : nPopularSeriesResults * 300
      "
      v-model="sliderPositionSeries"
      @input="changePositionSeries"
      class="slider"
      id="myRange"
    />
  </div>
</template>

<script>
import ResultCard from './ResultCard'

export default {
  name: 'PopularTrend',
  data() {
    return {
      sliderPositionFilms: 0,
      positionFilms: 0,
      sliderPositionSeries: 0,
      positionSeries: 0,
      viewportWidth: window.innerWidth - 60, //Slider margin
    }
  },
  components: {
    ResultCard,
  },
  props: {
    filteredPopularFilms: Array,
    filteredPopularSeries: Array,
    nPopularFilmsResults: Number,
    nPopularSeriesResults: Number,
  },
    computed: {
    computedPositionFilms() {
      return this.positionFilms
    },
    computedPositionSeries() {
      return this.positionSeries
    },
    computedFilmsMax() {
      return this.nPopularFilmsResults
    },
    computedSeriesMax() {
      return this.nPopularSeriesResults
    },
    computedViewport() {
      return this.viewport
    },
  },
  methods: {
    changePositionFilms() {
      this.positionFilms = this.sliderPositionFilms * -1
    },
    changePositionSeries() {
      this.positionSeries = this.sliderPositionSeries * -1
    },
    onResize() {
      this.viewportWidth = window.innerWidth-60//Slider margin
      this.sliderPositionFilms=1
      this.positionFilms=this.sliderPositionFilms
      this.sliderPositionSeries=1
      this.positionSeries=this.sliderPositionSeries
    },
  },
  mounted() {
    this.$nextTick(() => {
      window.addEventListener('resize', this.onResize)
    })
  },
  beforeDestroy() {
    window.removeEventListener('resize', this.onResize)
  },

}
</script>
<style scoped lang="scss">
@import '@/style/response-api-sliders.scss';
</style>
