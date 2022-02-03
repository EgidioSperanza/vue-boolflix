<template>
  <div>
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

<style scoped lang="scss"></style>
