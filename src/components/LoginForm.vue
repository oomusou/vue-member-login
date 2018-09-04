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
      <p></p>
    </form>
  </div>
</template>

<script>
import axios from 'axios';

// Client Proxy
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

      const body = () => ({
        username: this.username,
        password: this.password,
      });

      const processResponse = (response) => {
        this.loading = false;

        if (response.data.success) {
          this.loggedUser = response.data.username;
        } else {
          this.error = 'Incorrect username/password';
        }
      };

      const error = e => console.log(e);

      axios
        .post(`${API}/api/login`, body())
        .then(processResponse)
        .catch(error);
    },
  },
};
</script>

<style scoped>

</style>
