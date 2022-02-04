<template>
  <div>
    <h1>I Films più visti del momento!!!</h1>
    <div class="main-container">
      <div
        class="result-card"
        v-for="film in filteredPopularFilms"
        :key="film.id"
      >
        <result-card :result="film" />
      </div>
    </div>

    <h1>Le Serie Tv più amate del momento!!!</h1>
    <div class="main-container">
      <div
        class="result-card"
        v-for="serie in filteredPopularSeries"
        :key="serie.id"
      >
        <result-card :result="serie" />
      </div>
    </div>
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
  },
}
</script>
<style scoped lang="scss">
@import '@/style/variables.scss';

.main-container {
  margin: 20px;
  display: flex;
  overflow-x: scroll;

  .result-card {
    min-width: 300px;
    height: 450px;
    cursor: pointer;
    margin-bottom: 20px;
  }
}
</style>
