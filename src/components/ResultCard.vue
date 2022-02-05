<template>
  <div class="search-result">
    <div class="card">
      <p>
        Titolo:
        <span v-if="result.title">{{ result.title }}</span>
        <span v-else>{{ result.name }}</span>
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
        <span v-else>{{ result.original_language }}</span>
      </p>
      <p>
        Voto:
        <span v-if="(result.vote_average === '' || result.vote_average === 0)">
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
        <button v-if="!isShowOverview" @click="overviewStatus">
          Show overview
        </button>
        <div v-else>
          <p>{{ result.overview }}</p>
          <button @click="overviewStatus">Hide overview</button>
        </div>
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
      <span v-if="result.title">{{ result.title }}</span>
      <span v-else>{{ result.name }}</span>
    </div>
  </div>
</template>

<script>
export default {
  name: 'ResultCard',
  data() {
    return {
      languageFlag: `./flags/${this.result.original_language}.png`,
      isShowOverview: false,
    }
  },
  props: {
    result: Object,
  },
  methods: {
    voteConversion(vote) {
      let convertedVote = Math.round(vote / 2)
      return convertedVote
    },
    overviewStatus() {
      this.isShowOverview = !this.isShowOverview
    },
  },
}
</script>

<style scoped lang="scss">
@import '@/style/result-card.scss';

</style>
