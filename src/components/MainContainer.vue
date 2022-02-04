<template>
  <div v-if="filteredSearchList.length !== 0" class="main-container">
    <div class="film-card" v-for="film in filteredSearchList" :key="film.id">
      <film-card :film="film" />
    </div>
  </div>
  <div v-else>
    <h1>{{ msgNoResult }}</h1>
  </div>
</template>

<script>
import FilmCard from './FilmCard'

export default {
  name: 'MainContainer',
  components: {
    FilmCard,
  },
  props: {
    filteredSearchList: Array,
    msgNoResult: String,
  },
}
</script>
<style scoped lang="scss">
@import '@/style/variables.scss';

.main-container {
  margin: 0 auto;
  width: 90%;
  // width: 1300px;//xl
  // width: 990px;//lg
  // width: 620px;//md
  // width: 320px;//XS
  padding: 20px;
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;

  .film-card {
    width: 300px;
    height: 450px;
    cursor: pointer;
    margin-bottom: 20px;
  }
}
</style>
