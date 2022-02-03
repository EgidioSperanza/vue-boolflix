<template>
  <div class="main-container">
    <div v-for="film in filteredFilmsList" :key="film.id">
      <film-card :film="film" />
    </div>
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
    filteredFilmsList:Array
  },
}
</script>
<style scoped lang="scss">
.main-container{
  width: 100%;
  padding: 20px;
  display: flex;
  flex-wrap: wrap;
  gap:20px;
  justify-content: center;
}
</style>
