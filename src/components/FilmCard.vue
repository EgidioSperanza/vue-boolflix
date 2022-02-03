<template>
  <div class="film">
    <div class="card">
      <p>
        Titolo:
        <span>{{ film.title }}</span>
      </p>
      <p>
        Titolo Originale:
        <span>{{ film.original_title }}</span>
      </p>
      <p>
        Lingua:
        <span>{{ film.original_language }}</span>
      </p>
      <p>
        Voto:
        <i v-for="index in Math.round(film.vote_average / 2)" :key="index" class="fas fa-star"></i>
      </p>
      <div v-if="film.overview!==''">
        <p>Recensione:</p>
        <p>{{ film.overview }}</p>
      </div>
      <p v-else>Nessuna recensione disponibile</p>
    </div>
    <img
      v-if="film.poster_path !== null"
      :src="`https://www.themoviedb.org/t/p/original${film.poster_path}`"
      alt=""
    />
    <div v-else class="no-poster">
      <span>Locandina Assente per</span>
      <span>{{ film.title }}</span>
    </div>
  </div>
</template>

<script>
export default {
  name: 'FilmCard',
  props: {
    film: Object,
  },
}
</script>

<style scoped lang="scss">
@import '@/style/variables.scss';

.film {
  position: relative;
  width: 300px;
  height: 450px;
  cursor: pointer;

  &:hover .card {
    display: block;
  }
  .card {
    display: none;
    width: 100%;
    height: 100%;
    position: absolute;
    background-color: $card-background;
    color: $text-color;
    padding: 20px;
    overflow: auto;

    p {
      margin-bottom: 5px;

      i{
        color:$golden-color;
        font-size: 20px;
        margin: 0 5px;
      }
    }
  }
  img {
    width: 100%;
    object-fit: cover;
  }
  .no-poster {
    width: 100%;
    height: 100%;
    border: 1px solid $card-background;
    display: flex;
    flex-direction: column;
    justify-content: center;
    text-align: center;
    background-color: $no-poster-background;

    span {
      font-size: 30px;
    }
  }
}
</style>
