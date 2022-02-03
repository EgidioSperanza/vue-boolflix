<template>
  <header>
    <h1>Boolflix</h1>
    <div class="search">
      <label for="family-filter">
        <span v-if="familyFilterIsActivated">Family Filter On</span>
        <span v-else class="off">Family Filter Off</span>
      </label>
      <input
        type="checkbox"
        name="family-filter"
        checked
        @click="familyFilter()"
      />
      <div>
        <input
          type="text"
          placeholder="Search Title"
          v-model="stringToSearch"
          @keyup.enter="searchFilm"
        />
        <button @click="searchFilm">
          Search
        </button>
      </div>
    </div>
  </header>
</template>

<script>
export default {
  name: 'HeaderApp',
  data() {
    return {
      stringToSearch: '',
      familyFilterIsActivated: true,
    }
  },
  props: {},
  methods: {
    searchFilm() {
      this.$emit(
        'searchByTitle',
        this.stringToSearch,
        this.familyFilterIsActivated,
      )
    },
    familyFilter() {
      this.familyFilterIsActivated = !this.familyFilterIsActivated
      if (this.stringToSearch !== '') this.searchFilm()
    },
  },
}
</script>

<style scoped lang="scss">
@import '@/style/variables.scss';
header {
  min-height: 100px;
  background-color: $header-color;
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
  align-items: center;
  padding: 20px;
  h1 {
    color: $title-color;
    text-transform: uppercase;
    font-size: 50px;
  }
  .search {
    display: flex;
    flex-wrap: wrap;
    align-items: center;
    label {
      margin: 10px 0;
      color: $text-color;
    }
    input[type='checkbox'] {
      margin: 10px 20px;
    }
    input[type='text'] {
      height: 50px;
      width: 200px;
    }
    button {
      height: 50px;
      margin-left: 20px;
      padding: 0 20px;
    }
    .off {
      color: $off-text-color;
    }
  }
}
</style>
