<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <!-- create action -->
    <div>
      <h2>New Movie</h2>
      Title:
      <input type="text" v-model="newMovieTitle" />
      <br />
      Year:
      <input type="text" v-model="newMovieYear" />
      <br />
      Director:
      <input type="text" v-model="newMovieDirector" />
      <br />
      Plot:
      <input type="text" v-model="newMoviePlot" />
      <br />
      English:
      <input type="text" v-model="newMovieEnglish" />
      <br />
      <button v-on:click="createMovie()">Add Movie</button>
    </div>
    <!-- indes/show action -->
    <div v-for="movie in movies">
      <h2>Title: {{ movie.title }}</h2>
      <h3>Year: {{ movie.year }}</h3>
      <button v-on:click="movie.showExtraInfo = !movie.showExtraInfo">More Info</button>
      <div v-if="movie.showExtraInfo">
        <h3>Director: {{ movie.director }}</h3>
        <p>Plot: {{ movie.plot }}</p>
        <div>
          <!-- update action -->
          <h4>Edit Movie</h4>
          Title:
          <input type="text" v-model="movie.title" />
          <br />
          Year:
          <input type="text" v-model="movie.year" />
          <br />
          Director:
          <input type="text" v-model="movie.director" />
          <br />
          Plot:
          <input type="text" v-model="movie.plot" />
          <br />
          English:
          <input type="text" v-model="movie.english" />
          <br />
          <button v-on:click="updateMovie(movie)">Update</button>
          <button v-on:click="destroyMovie(movie)">Delete</button>
        </div>
      </div>
    </div>
  </div>
</template>

<style></style>

<script>
// var axios = require("axios");
import axios from "axios"; //don't forget to add this!!!
export default {
  data: function() {
    return {
      message: "Welcome to Zen's Movie App!",
      movies: [],
      newMovieTitle: "",
      newMovieYear: null,
      newMovieDirector: "",
      newMoviePlot: "",
      newMovieEnglish: true
    };
  },
  //response = HTTP.get("/api/movies")
  //@recipes = response.parse
  created: function() {
    axios.get("/api/movies").then(response => {
      response.data.forEach(movie => {
        movie.showExtraInfo = false;
      });
      console.log(response.data);
      this.movies = response.data;
    });
  },
  methods: {
    createMovie: function() {
      var params = {
        title: this.newMovieTitle,
        year: this.newMovieYear,
        director: this.newMovieDirector,
        plot: this.newMoviePlot,
        english: this.newMovieEnglish
      };
      axios
        .post("/api/movies", params)
        .then(response => {
          console.log("Success!", response.data);
          this.movies.push(response.data);
        })
        .catch(error => {
          console.log(error.response.data.errors);
        });
    },
    updateMovie: function(movie) {
      var params = {
        title: movie.title,
        year: movie.year,
        director: movie.director,
        plot: movie.plot,
        english: movie.english
      };
      axios
        .patch(`/api/movies/${movie.id}`, params)
        .then(response => {
          console.log("Success!", response.data);
        })
        .catch(error => {
          console.log(error.response.data.errors);
        });
    },
    destroyMovie: function(movie) {
      axios.delete(`/api/movies/${movie.id}`).then(response => {
        console.log("Success!", response.data);
        // find index of movie object in movie array
        var index = this.movies.indexOf(movie);
        // remove that index from the recipes array
        this.movies.splice(index, 1);
      });
    }
  }
};
</script>
