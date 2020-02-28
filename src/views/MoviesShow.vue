<template>
  <div class="movies-show">
    <h2>{{ movie.title }}</h2>
    <img v-bind:src="movie.image_url" alt="" />
    <h3>Released: {{ movie.year }}</h3>
    <h4>Director: {{ movie.director }}</h4>
    <p>{{ movie.plot }}</p>
    <br />

    movies user id {{ movie.user_id }}
    <br />
    current user id {{ $parent.getUserId() }}
    <br />

    <router-link v-if="movie.user_id == $parent.getUserId()" v-bind:to="`/movies/${movie.id}/edit`">
      Edit Movie
    </router-link>
  </div>
</template>

<style></style>

<script>
import axios from "axios";

export default {
  data: function() {
    return {
      movie: {}
    };
  },

  created: function() {
    axios.get(`/api/movies/${this.$route.params.id}`).then(response => {
      console.log(response.data);
      this.movie = response.data;
    });
  },

  methods: {}
};
</script>
