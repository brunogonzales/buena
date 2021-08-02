<template>
  <main>
    <input type="search" v-model="q" />
    <div class="flex flex-col space-y-4 px-2 py-2">
      <div v-for="movie in filteredMovies" :key="movie.id">
        <div class="space-y-1">
          <h2 class="text-lg">{{ movie.fullTitle }}</h2>
          <p class="font-mono">{{ movie.crew }}</p>
          <img class="max-h-32" :src="movie.image" :alt="movie.title" />
          <div class="flex space-x-2">
            <img src="~/assets/imdb-icon.svg" alt="imdb" /><span
              >{{ movie.imDbRating }}/10</span
            >
          </div>
        </div>
      </div>
    </div>
  </main>
</template>

<script>
export default {
  data() {
    return {
      movies: [],
      q: ""
    };
  },

  computed: {
    filteredMovies() {
      return this.movies.filter(movie =>
        (movie.crew + movie.fullTitle).toLowerCase().includes(this.q)
      );
    }
  },

  async asyncData({ $axios }) {
    const movies = await $axios.$get(
      `https://imdb-api.com/en/API/Top250Movies/${process.env.IMDB_API_KEY}`
    );
    return { movies: movies.items };
  }
};
</script>
