<template>
  <div>
    <h1 v-if="loggedUser">Welcome {{ loggedUser }}</h1>
    <form v-else @submit.prevent="onSubmit">
      <label>Username</label>
      <input type="text" v-model="username" :disabled="loading">
      <p></p>
      <label>Password</label>
      <input type="password" v-model="password" :disabled="loading">
      <p></p>
      <button type="submit" :disabled="loading">Submit</button>
      <p></p>
      <span v-if="error">{{ error }}</span>
      <span v-if="loggedUser">{{ loggedUser }}</span>
      <p></p>
    </form>
  </div>
</template>

<script>
import axios from 'axios';

// Server CORS
// const API = 'https://localhost:5001';
// http-proxy-middleware CORS
const API = '';
export default {
  name: 'LoginForm',
  data() {
    return {
      username: '',
      password: '',
      loggedUser: '',
      error: '',
      disabled: false,
      loading: false,
    };
  },
  watch: {
    username() {
      this.error = '';
    },
    password() {
      this.error = '';
    },
  },
  methods: {
    onSubmit() {
      this.loading = true;

      axios.post(`${API}/api/login`, {
        username: this.username,
        password: this.password,
      }).then((response) => {
        this.loading = false;

        if (response.data.success) {
          this.loggedUser = response.data.username;
        } else {
          this.error = 'Incorrect username/password';
        }
      });
    },
  },
};
</script>

<style scoped>

</style>
