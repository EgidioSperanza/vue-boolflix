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
        <span>
          <vote-average :vote="result.vote_average" />
        </span>
      </p>
      <div
        :class="!isShowOverview ? 'overview' : ''"
        v-if="result.overview !== ''"
      >
        <p>Recensione:</p>
        <button class="card-btn" v-if="!isShowOverview" @click="overviewStatus">
          Show overview
        </button>
        <div v-else>
          <p>
            {{ result.overview }}
            <button class="card-btn" @click="overviewStatus">
              Hide overview
            </button>
          </p>
        </div>
      </div>
      <p v-else>Nessuna recensione disponibile</p>
      <p>Cast:</p>
      <div v-for="cast in resultCast" :key="cast.id">
        <div v-if="cast.id === result.id">
          <div v-for="actor in cast.cast" :key="actor.id">
            <p v-if="actor.order <= 5">
              {{ actor.name }}
              <span v-if="actor.character">
                nel ruolo di {{ actor.character }}
              </span>
            </p>
          </div>
        </div>
      </div>
      <div v-for="cast in resultTrendCast" :key="cast.id">
        <div v-if="cast.id === result.id">
          <div v-for="actor in cast.cast" :key="actor.id">
            <p v-if="actor.order <= 5">
              {{ actor.name }} nel ruolo di {{ actor.character }}
            </p>
          </div>
        </div>
      </div>
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
import VoteAverage from './VoteAverage.vue'

export default {
  name: 'ResultCard',
  components: {
    VoteAverage,
  },
  data() {
    return {
      languageFlag: `./flags/${this.result.original_language}.png`,
      isShowOverview: false,
    }
  },
  props: {
    result: Object,
    resultCast: Array,
    resultTrendCast: Array,
  },
  methods: {
    overviewStatus() {
      this.isShowOverview = !this.isShowOverview
    },
  },
}
</script>

<style scoped lang="scss">
@import '@/style/result-card.scss';
</style>
