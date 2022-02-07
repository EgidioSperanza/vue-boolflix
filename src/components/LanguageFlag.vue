<template>
  <div>
    <p class="flags">
      Lingua:
      <img
        v-if="languageFlag !== null"
        :src="languageFlag"
        :alt="language"
      />
      <span v-else>{{language }}</span>
    </p>
  </div>
</template>
<script>
export default {
  name: 'LanguageFlag',
  data() {
    return {
      languageFlag: `./flags/${this.language}.png`,
    }
  },
  props: {
    language: String,
  },
  methods: {},
}
</script>

<style scoped lang="scss">

  p.flags {
    display: flex;
    align-items: center;
    > img {
      width: 30px;
      height: 20px;
      margin-left: 5px;
    }
  }
</style>
