<template>
  <div v-if="filteredFilmsList.length !== 0 || filteredSeriesList.length !== 0">
    <h1>La tua Ricerca ha prodotto i seguenti risultati!!!</h1>
    <div class="main-container">
      <div class="result-card" v-for="film in filteredFilmsList" :key="film.id">
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
