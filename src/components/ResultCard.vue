<template>
  <div class="search-result">
    <div class="card">
      <p>
        Titolo:
        <span v-if="result.title">{{ result.title }}</span>
        <span v-else>{{ result.name }}</span>
      </p>
      <p
        v-if="
          result.title !== result.original_title ||
          result.name !== result.original_name
        "
      >
        Titolo Originale:
        <span v-if="result.original_title">{{ result.original_title }}</span>
        <span v-else>{{ result.original_name }}</span>
      </p>
      <language-flag :language="result.original_language" />
      <vote-average :vote="result.vote_average" />
      <div
        :class="!isShowOverview ? 'overview' : ''"
        v-if="result.overview !== ''"
      >
        <p>Recensione:</p>
        <button class="card-btn" v-if="!isShowOverview" @click="overviewStatus">
          Show overview
        </button>
        <div v-else>
          <p class="overview-text">
            {{ result.overview }}
            <button class="card-btn hide-overview" @click="overviewStatus">
              Hide overview
            </button>
          </p>
        </div>
      </div>
      <p v-else>Nessuna recensione disponibile</p>
      <div v-for="cast in resultCast" :key="cast.id">
        <div v-if="cast.id === result.id">
          <div v-for="actor in cast.cast" :key="actor.id">
            <p v-if="actor.order <= 5">
              <span :class="actor.order % 2 === 0 ? 'alternate-cast' : ''">
                {{ actor.name }}
              </span>
            </p>
          </div>
        </div>
      </div>
      <div v-for="cast in resultTrendCast" :key="cast.id">
        <div v-if="cast.id === result.id">
          <div v-for="actor in cast.cast" :key="actor.id">
            <p v-if="actor.order <= 5">
              <span :class="actor.order % 2 === 0 ? 'alternate-cast' : ''">
                {{ actor.name }}
              </span>
            </p>
          </div>
        </div>
      </div>
      <div class="more-info">
        <button @click="$emit('thisResultInfo', result)">Vai alla scheda Info</button>
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
import LanguageFlag from './LanguageFlag.vue'

export default {
  name: 'ResultCard',
  components: {
    VoteAverage,
    LanguageFlag,
  },
  data() {
    return {
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
