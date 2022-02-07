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
    <div class="poster-and-actors">
      <div class="poster" v-if="result.poster_path !== null">
        <img
          :src="`https://www.themoviedb.org/t/p/original${result.poster_path}`"
          :alt="
            result.title
              ? `Locandina di ${result.title}`
              : `Locandina di ${result.name}`
          "
        />
      </div>
      <div v-else class="no-poster">
        <span>Locandina Assente per</span>
        <span v-if="result.title">{{ result.title }}</span>
        <span v-else>{{ result.name }}</span>
      </div>
      <div v-for="cast in resultCast" :key="cast.id">
        <div v-if="cast.id === result.id">
          <div v-for="actor in cast.cast" :key="actor.id">
            <div class="actor-profile" v-if="actor.order <= 5">
              <div class="profile-photo" v-if="actor.profile_path !== null">
                <img
                  :src="`https://www.themoviedb.org/t/p/w45${actor.profile_path}`"
                  :alt="actor.name"
                />
                <!-- TODO: hover w185 -->
              </div>
              <p class="actor-name">
                {{ actor.name }}
                <span v-if="actor.character">
                  nel ruolo di {{ actor.character }}
                </span>
              </p>
            </div>
          </div>
        </div>
      </div>
      <div v-for="cast in resultTrendCast" :key="cast.id">
        <div v-if="cast.id === result.id">
          <div v-for="actor in cast.cast" :key="actor.id">
            <div class="actor-profile" v-if="actor.order <= 4">
              <div class="profile-photo" v-if="actor.profile_path !== null">
                <img
                  :src="`https://www.themoviedb.org/t/p/w45${actor.profile_path}`"
                  :alt="actor.name"
                />
                <!-- TODO: hover w185 -->
              </div>
              <p class="actor-name">
                {{ actor.name }}
                <span v-if="actor.character">
                  nel ruolo di {{ actor.character }}
                </span>
              </p>
            </div>
          </div>
        </div>
      </div>
    </div>
    <div class="language-and-vote">
      <language-flag :language="result.original_language" />
      <vote-average :vote="result.vote_average" />
    </div>

    <div class="overview" v-if="result.overview !== ''">
      <p>Recensione:</p>
      <p class="overview-text">
        {{ result.overview }}
      </p>
    </div>
    <p v-else>Nessuna recensione disponibile</p>
    <button @click="$emit('goBack')">Torna indietro</button>
  </div>
</template>

<script>
import VoteAverage from './VoteAverage.vue'
import LanguageFlag from './LanguageFlag.vue'

export default {
  name: 'SingleCardInfo',
  components: {
    VoteAverage,
    LanguageFlag,
  },
  props: {
    result: Object,
    resultCast: Array,
    resultTrendCast: Array,
  },
  methods: {},
}
</script>

<style scoped lang="scss">
@import '@/style/single-card.scss';
</style>
