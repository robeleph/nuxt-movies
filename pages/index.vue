<template>
  <div class="home">
    <!-- Hero -->
    <Hero />
    <!-- Search -->
    <Search @search-movie="searchedMoviesMethod" @clear-input="clearSearch" />
    <!-- Loading -->
    <Loading v-if="$fetchState.pending" />
    <!-- Movies -->
    <div class="container movies">
      <div id="movie-grid" class="movies-grid">
        <Movie
          v-for="movie in movies"
          :key="movie.id"
          :movie="movie"
        />
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'IndexPage',
  data() {
    return {
      movies: [],
      searchedMovies: [],
      searchMoviesHolder: [],
    }
  },

  async fetch() {
    await this.getMovies()
  },
  head() {
    return {
      title: 'Movie App - Latest Streaming Movie Info',
      meta: [
        {
          hid: 'description',
          name: 'description',
          content: 'Get all the latest streaming movies in theaters & online',
        },
      ],
    }
  },
  fetchDelay: 1000,
  methods: {
    async getMovies() {
      const data = axios.get(
        `https://api.themoviedb.org/3/movie/now_playing?api_key=3899c716239055d520856c4803dfff41&language=en-US&page=1`
      )
      const result = await data
      result.data.results.forEach((movie) => {
        this.movies.push(movie)
      })
    },
    async searchedMoviesMethod(searchInput) {
      this.searchMoviesHolder = this.movies
      this.movies = []
      const data = axios.get(
        `https://api.themoviedb.org/3/search/movie?api_key=3899c716239055d520856c4803dfff41&language=en-US&page=1&query=${searchInput}`
      )
      const result = await data
      result.data.results.forEach((movie) => {
        this.movies.push(movie)
      })
    },
    clearSearch() {
      this.movies = this.searchMoviesHolder
    },
  },
}
</script>
<style lang="scss">
.home {
  .movies {
    padding: 32px 16px;
    .movies-grid {
      display: grid;
      column-gap: 32px;
      row-gap: 64px;
      grid-template-columns: 1fr;
      @media (min-width: 500px) {
        grid-template-columns: repeat(2, 1fr);
      }
      @media (min-width: 750px) {
        grid-template-columns: repeat(2, 1fr);
      }
      @media (min-width: 1100px) {
        grid-template-columns: repeat(4, 1fr);
      }
      .movie {
        position: relative;
        display: flex;
        flex-direction: column;
      }
    }
  }
}
</style>
