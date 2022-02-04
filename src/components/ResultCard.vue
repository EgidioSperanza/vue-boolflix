<template>
  <div class="search-result">
      <div class="card">
        <p>
          Titolo:
          <span v-if="result.title">{{ result.title }}</span>
          <span v-else>{{result.name}}</span>
        </p>
        <p>
          Titolo Originale:
          <span v-if="result.original_title">{{ result.original_title }}</span>
          <span v-else>{{ result.original_name }}</span>
        </p>
        <p class="flags">
          Lingua:
          <img
            v-if="languageFlag !== null"
            :src="`${languageFlag}`"
            :alt="result.original_language"
          />
          <span v-else> {{ result.original_language }}</span>
        </p>
        <p>
          Voto:
          <span v-if="(result.vote_average = '' || result.vote_average === 0)">
            Nessun Voto da Mostrare
          </span>
          <i
            v-else
            v-for="index in voteConversion(result.vote_average)"
            :key="index"
            class="fas fa-star"
          ></i>
        </p>
        <div v-if="result.overview !== ''">
          <p>Recensione:</p>
          <p>{{ result.overview }}</p>
        </div>
        <p v-else>Nessuna recensione disponibile</p>
      </div>
      <img
        v-if="result.poster_path !== null"
        :src="`https://www.themoviedb.org/t/p/original${result.poster_path}`"
        alt=""
      />
      <div v-else class="no-poster">
        <span>Locandina Assente per</span>
        <span>{{ result.title }}</span>
      </div>
  </div>
</template>

<script>
export default {
  name: 'ResultCard',
  data() {
    return {
      languageFlag: `./flags/${this.result.original_language}.png`,
    }
  },
  props: {
    result: Object,
  },
  methods: {
    voteConversion(vote){
      return Math.round(vote/2)
    }
  },
}
</script>

<style scoped lang="scss">
@import '@/style/variables.scss';

.search-result {
  position: relative;
  width: 100%;
  height: 100%;

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

      i {
        color: $golden-color;
        font-size: 20px;
        margin: 0 5px;
      }
    }
  }
  p.flags {
    display: flex;
    align-items: center;
    > img {
      width: 30px;
      height: 20px;
      margin-left: 5px;
    }
  }
  img {
    width: 100%;
    height: 100%;
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
