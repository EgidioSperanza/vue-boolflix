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
  </div>
</template>

<script>
export default {
  name: 'Popular',
  props: {
    filteredPopularFilms: Array,
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
