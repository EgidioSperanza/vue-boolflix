<template>
  <div>
    <h1>I Films più visti del momento!!!</h1>
    <div class="main-container">
      <div class="slider" :style="{ left: position[1] + 'px' }">
        <div
          class="result-card"
          v-for="film in filteredPopularFilms"
          :key="film.id"
        >
          <result-card :result="film" @thisResultInfo="thisResultInfo"/>
        </div>
      </div>
    </div>
    <input
      type="range"
      min="1"
      :max="
        nResults[1] * 300 > viewportWidth
          ? nResults[1] * 300 - viewportWidth
          : nResults[1] * 300
      "
      v-model="sliderPosition[1]"
      @input="$emit('changePosition', 1)"
      class="slider"
      id="myRange"
    />

    <h1>Le Serie Tv più amate del momento!!!</h1>
    <div class="main-container">
      <div class="slider" :style="{ left: position[2] + 'px' }">
        <div
          class="result-card"
          v-for="serie in filteredPopularSeries"
          :key="serie.id"
        >
          <result-card :result="serie" @thisResultInfo="thisResultInfo"/>
        </div>
      </div>
    </div>
    <input
      type="range"
      min="1"
      :max="
        nResults[2] * 300 > viewportWidth
          ? nResults[2] * 300 - viewportWidth
          : nResults[2] * 300
      "
      v-model="sliderPosition[2]"
      @input="$emit('changePosition', 2)"
      class="slider"
      id="myRange"
    />
  </div>
</template>

<script>
import ResultCard from './ResultCard'

export default {
  name: 'PopularTrend',
  components: {
    ResultCard,
  },
  props: {
    filteredPopularFilms: Array,
    filteredPopularSeries: Array,
    resultTrendCast:Array,
    nResults: Array,
    position: Array,
    sliderPosition: Array,
    viewportWidth: Number,
  },
  methods: {
    thisResultInfo(result){
      this.$emit('thisResultInfo', result)
    },
  },
}
</script>
<style scoped lang="scss">
@import '@/style/response-api-sliders.scss';
</style>
