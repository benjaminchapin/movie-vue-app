<template>
  <div class="movies-new">
    <div class="container">
      <form v-on:submit.prevent="createMovie()">
        <h1>Add Movie</h1>
        <ul>
          <li class="text-danger" v-for="error in errors">{{ error }}</li>
        </ul>
        <div class="form-group">
          <label>Title:</label>
          <input type="text" class="form-control" v-model="newTitle" />
        </div>
        <br />
        <div class="form-group">
          <label>Year:</label>
          <input type="text" class="form-control" v-model="newYear" />
        </div>
        <br />
        <div class="form-group">
          <label>Director:</label>
          <input type="text" class="form-control" v-model="newDirector" />
        </div>
        <br />
        <div class="form-group">
          <label>Plot:</label>
          <input type="text" class="form-control" v-model="newPlot" />
          <br />
          <small v-if="newPlot" v-bind:class="{ current: newPlot.length > 20 }">
            Character count {{ 20 - newPlot.length }}
          </small>
        </div>
        <br />
        <div class="form-group">
          <label>Box Cover URL:</label>
          <input type="text" class="form-control" v-model="newImage_url" />
        </div>
        <input type="submit" class="btn btn-primary" value="Submit" />
      </form>
    </div>
  </div>
</template>

<style>
.current {
  color: red;
}
</style>

<script>
import axios from "axios";
export default {
  data: function() {
    return {
      newTitle: "",
      newYear: "",
      newDirector: "",
      newPlot: "",
      newImage_url: "",
      errors: []
    };
  },
  created: function() {},
  methods: {
    createMovie: function() {
      var params = {
        title: this.newTitle,
        year: this.newYear,
        director: this.newDirector,
        plot: this.newPlot,
        image_url: this.newImage_url
      };
      axios
        .post("/api/movies", params)
        .then(response => {
          this.$router.push("/movies");
        })
        .catch(error => {
          console.log(error.response);
          this.errors = error.response.data.errors;
        });
    }
  }
};
</script>
