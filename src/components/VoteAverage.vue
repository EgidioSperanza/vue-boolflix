<template>
  <div>
    <p>
      Voto:
      <span v-if="(vote === '' || vote === 0)">
        Nessun Voto da Mostrare
      </span>
      <span v-else>
        <i
          v-for="index in voteConversion(this.vote)"
          :key="index"
          class="fas fa-star"
        ></i>
      </span>
    </p>
  </div>
</template>
<script>
export default {
  name: 'VoteAverage',
  props: {
    vote: Number,
  },
  methods: {
    voteConversion() {
      return Math.round(this.vote / 2)
    },
  },
}
</script>

<style scoped lang="scss">
@import '@/style/variables.scss';
    p {
      margin-bottom: 10px;
    }

i {
  color: $golden-color;
  font-size: 20px;
  margin: 0 5px;
}
</style>
