<template>
  <header>
    <h1>
      B
      <i class="fas fa-tv"></i>
      <i class="fas fa-tv"></i>
      lflix
    </h1>
    <div class="search">
      <div v-if="!queries.include_adult">
        <span>Family Filter On</span>
        <i class="fas fa-check-square" @click="familyFilter"></i>
      </div>
      <div v-else>
        <span class="off">Family Filter Off</span>
        <i class="fas fa-square" @click="familyFilter"></i>
      </div>
      <div>
        <input
          type="text"
          placeholder="Search Title"
          v-model="queries.query"
          @keyup.enter="searchResult"
        />
        <button @click="searchResult">
          <i class="fas fa-search"></i>
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
      if (this.queries.query !== '') {
        this.searchResult()
      }
    },
  },
}
</script>

<style scoped lang="scss">
@import '@/style/variables.scss';
header {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
  align-items: center;
  background-color: $header-color;

  h1 {
    font-size: 50px;
  }

  .search {
    margin-right: 10px;
    display: flex;
    align-items: center;

    > div > span,
    > div > i {
      margin-right: 10px;
    }

    > div > span {
      color: $text-color;
      &.off {
        color: $off-text-color;
      }
    }

    > div > i{
      color:$input-border-col;
      cursor: pointer;
    }

    input[type='text'] {
      margin-right: 10px;
      height: 50px;
      background-color: $bkg-input;
      padding-left: 5px;
      border: 1px solid $input-border-col;
      color: $input-text-color;
    }

    button {
      width: 25px;
      height: 25px;
      background-color: $bkg-color-btn;
      border:1px solid $button-border-color;
      cursor:pointer;
    }
  }
}
</style>
