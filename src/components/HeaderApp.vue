<template>
  <header>
    <h1>
      B
      <i class="fas fa-tv"></i>
      <i class="fas fa-tv"></i>
      lflix
    </h1>
    <div class="search">
      <label for="family-filter">
        <span v-if="!queries.include_adult">Family Filter On</span>
        <span v-else class="off">Family Filter Off</span>
      </label>
      <input
        type="checkbox"
        name="family-filter"
        checked
        @click="familyFilter"
      />
      <div>
        <input
          type="text"
          placeholder="Search Title"
          v-model="queries.query"
          @keyup.enter="searchResult"
        />
        <button @click="searchResult">
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
      queries: {
        query: '',
        include_adult: false,
      },
    }
  },
  props: {},
  methods: {
    searchResult() {
      this.$emit('searchResult', this.queries)
    },
    familyFilter() {
      this.queries.include_adult = !this.queries.include_adult
      this.searchResult()
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
  padding: 20px;
  h1 {
    color: $title-color;
    text-transform: uppercase;
    font-size: 50px;

    i {
      font-size: 40px;
    }
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
      width: 170px;
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
