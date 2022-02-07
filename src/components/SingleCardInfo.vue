<template>
  <div class="single-card-info">
    <h1>
      <span v-if="result.title">{{ result.title }}</span>
      <span v-else>{{ result.name }}</span>
    </h1>
    <h2
      v-if="
        result.original_title !== result.title ||
        result.original_name !== result.name
      "
    >
      <span v-if="result.original_title">{{ result.original_title }}</span>
      <span v-else>{{ result.original_name }}</span>
    </h2>
    <div class="poster" v-if="result.poster_path !== null">
      <img
        :src="`https://www.themoviedb.org/t/p/original${result.poster_path}`"
        alt=""
      />
    </div>
    <div v-else class="no-poster">
      <span>Locandina Assente per</span>
      <span v-if="result.title">{{ result.title }}</span>
      <span v-else>{{ result.name }}</span>
    </div>
  </div>
</template>

<script>
// import VoteAverage from './VoteAverage.vue'
// import LanguageFlag from './LanguageFlag.vue'

export default {
  name: 'SingleCardInfo',
  components: {
    // VoteAverage,
    // LanguageFlag,
  },
  props: {
    result: Object,
  },
  methods: {},
}
</script>

<style scoped lang="scss">
@import '@/style/single-card.scss';
</style>
