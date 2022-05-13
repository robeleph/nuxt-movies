<template>
  <div class="container search">
    <input
      v-model="searchInput"
      class="search"
      type="text"
      placeholder="Search..."
      @change="searchInputMovie"
    />
    <button v-show="searchInput !== ''" class="button" @click="clearInput">
      Clear Search
    </button>
  </div>
</template>

<script>
export default {
  name: 'Search',
  data() {
    return {
      searchInput: '',
    }
  },
  methods: {
    searchInputMovie() {
      this.$emit('search-movie', this.searchInput)
    },
    clearInput() {
      this.searchInput = ""
      this.$emit('clear-input')
    },
  },
}
</script>

<style lang="scss">
.search {
  display: flex;
  padding: 32px 16px;
  input {
    max-width: 350px;
    width: 100%;
    padding: 12px 6px;
    font-size: 14px;
    border: none;
    &:focus {
      outline: none;
    }
  }
  .button {
    border-top-left-radius: 0;
    border-bottom-left-radius: 0;
  }
}
</style>
