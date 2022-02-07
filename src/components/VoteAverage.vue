<template>
  <div>
    <i
      v-for="index in voteConversion(this.vote)"
      :key="index"
      class="fas fa-star"
    ></i>
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

i {
  color: $golden-color;
  font-size: 20px;
  margin: 0 5px;
}
</style>
