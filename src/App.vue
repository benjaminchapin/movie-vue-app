<template>
  <div id="app">
    <div id="nav">
      <!-- <router-link to="/">Home</router-link> | -->
      <router-link v-if="isLoggedIn()" to="/movies">All Movies</router-link>
      |
      <router-link v-if="isLoggedIn()" to="/movies/new">Add Movies</router-link>
      |
      <router-link v-if="!isLoggedIn()" to="/signup">Signup</router-link>
      |
      <router-link v-if="!isLoggedIn()" to="/login">Login</router-link>
      |
      <router-link v-if="isLoggedIn()" to="/logout">Logout</router-link>
      |
    </div>
    <router-view />
  </div>
</template>

<style>
img {
  width: 250px;
}
</style>

<script>
export default {
  methods: {
    isLoggedIn: function() {
      if (localStorage.getItem("jwt")) {
        return true;
      } else {
        return false;
      }
    },
    getUserId: function() {
      return localStorage.getItem("user_id");
    }
  }
};
</script>
