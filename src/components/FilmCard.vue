<template>
  <div>
    {{film.original_title}}
  </div>
</template>

<script>

export default {
  name: 'FilmCard',
  props: {
    film:Object,
  }
}
</script>

<style scoped lang="scss">
</style>
