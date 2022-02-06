<template>
  <div
    class="wrapper"
    v-if="filteredFilmsList.length !== 0 || filteredSeriesList.length !== 0"
  >
    <h1>La tua Ricerca ha prodotto i seguenti risultati!!!</h1>
    <div class="main-container">
      <div class="slider" :style="{ left: position[0] + 'px' }">
        <div
          class="result-card"
          v-for="film in filteredFilmsList"
          :key="film.id"
        >
          <result-card :result="film" />
        </div>
        <div
          class="result-card"
          v-for="serie in filteredSeriesList"
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
        nResults[0] * 300 > viewportWidth ? nResults[0] * 300 - viewportWidth : nResults[0] * 300
      "
      v-model="sliderPosition[0]"
      @input="$emit('changePosition', 0)"
      class="slider"
      id="myRange"
    />
  </div>
  <div v-else>
    <h1 class="error">{{ msgNoResult }}</h1>
  </div>
</template>

<script>
import ResultCard from './ResultCard'

export default {
  name: 'MainContainer',
  components: {
    ResultCard,
  },
  props: {
    filteredFilmsList: Array,
    filteredSeriesList: Array,
    msgNoResult: String,
    nResults: Array,
    position:Array,
    sliderPosition:Array,
    viewportWidth:Number,
  },
}
</script>
<style scoped lang="scss">
@import '@/style/response-api-sliders.scss';
</style>
