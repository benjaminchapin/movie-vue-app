<template>
  <div class="movies-index">
    <div
      v-for="movie in movies"
      v-on:click="selectedMovie = movie"
      v-bind:class="{ selected: movie === selectedMovie }"
    >
      <h2>{{ movie.title }}</h2>
      <router-link :to="`/movies/${movie.id}`">
        <img v-bind:src="movie.image_url" alt="" />
        <br />
        <br />
      </router-link>
    </div>
  </div>
</template>

<style>
.selected {
  background-color: aqua;
  transition: background-color 1s ease;
}
</style>

<script>
import axios from "axios";
export default {
  data: function() {
    return {
      movies: [],
      selectedMovie: {}
    };
  },
  created: function() {
    axios.get("/api/movies").then(response => {
      this.movies = response.data;
    });
  },
  methods: {}
};
</script>
