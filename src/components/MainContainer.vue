<template>
  <div
    class="wrapper"
    v-if="filteredFilmsList.length !== 0 || filteredSeriesList.length !== 0"
  >
    <h1>La tua Ricerca ha prodotto i seguenti risultati!!!</h1>
    <div class="main-container">
      <div class="slider" :style="{ left: position + 'px' }">
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
        nResults * 300 > viewportWidth ? nResults * 300 - viewportWidth : nResults * 300
      "
      v-model="sliderPosition"
      @input="changePosition"
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
  data() {
    return {
      sliderPosition: 0,
      position: 0,
      viewportWidth: window.innerWidth-60,//Slider margin
    }
  },
  // //DEBUG
  // watch: {
  //   viewportWidth(newWidth, oldWidth) {
  //     console.log(`it changed to ${newWidth} from ${oldWidth}`) 
  //   },
  // },
  components: {
    ResultCard,
  },
  props: {
    filteredFilmsList: Array,
    filteredSeriesList: Array,
    msgNoResult: String,
    nResults: Number,
  },
  computed: {
    computedPosition() {
      return this.position
    },
    computedMax() {
      return this.nResults
    },
    computedViewport() {
      return this.viewport
    },
  },
  methods: {
    changePosition() {
      this.position = this.sliderPosition * -1
    },
    onResize() {
      this.viewportWidth = window.innerWidth-60//Slider margin
      this.sliderPosition=1
      this.position=this.sliderPosition
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
