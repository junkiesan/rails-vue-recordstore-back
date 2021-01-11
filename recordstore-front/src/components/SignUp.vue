<template>
  <div class="container">
    <div class="card">
      <h3 class="title"> Sign Up</h3>
      <form @submit.prevent="signup">
        <div class="text-red" v-if="error"> {{error}} </div>

        <div class="row">
          <label for="email" class="label">E-mail Adress</label>
          <input type="email" v-model="email" class="input" id="email" placeholder="john.doe@example.com">
        </div>

        <div class="row">
          <label for="password" class="label">Password</label>
          <input type="password" v-model="password" class="input" id="password" placeholder="**********">
        </div>

        <div class="row">
          <label for="password" class="label">Password Confirmation</label>
          <input type="password" v-model="password_confirmation" class="input" id="password_confirmation" placeholder="**********">
        </div>

        <button type="submit" class="button">
          Sign Up
        </button>

        <div class="my-4">
          <router-link to="/" class="link">Sign In</router-link>
        </div>

      </form>
    </div>
  </div>
</template>

<script>
export default {
  name: 'SignIn',
  data: function () {
    return {
      email: '',
      password: '',
      password_confirmation: '',
      error: ''
    }
  },
  created () {
    this.checkSignUp()
  },
  updated () {
    this.checkSignUp()
  },
  methods: {
    signIn: function () {
      this.$http.plain.post('/signup', { email: this.email, password: this.password, password_confirmation: this.password_confirmation })
        .then(response => this.signinSuccessful(response))
        .catch(error => this.signinFailed(error))
    },
    signinSuccessful: function (response) {
      if (!response.data.csrf) {
        this.signinFailed(response)
        return
      }
      localStorage.csrf = response.data.csrf
      localStorage.signedIn = true
      this.error = ''
      this.$router.replace('/records')
    },
    signinFailed: function (error) {
      this.error = (error.response && error.response.data && error.response.data.error) || ''
      delete localStorage.csrf
      delete localStorage.signedIn
    },
    checkSignIn: function () {
      if (localStorage.signedIn) {
        this.$router.replace('/records')
      }
    }
  }
}
</script>

<style>
  body {
    font-family: helvetica;
  }

  input {
    border-radius: 3px;
  }

  .container {
    display: flex;
    justify-content: center;
    align-items: center;
    margin-top: 10rem;
  }

  .card {
    border: 3px solid #e0e0e0;
    border-radius: 5px;
    padding: 5rem;
  }

  .title {
    font-weight: 300;
    font-family: helvetica;
    font-size: 36px;
  }

  .row {
    margin-bottom: 1rem;
  }

  .button {
    margin-bottom: 1rem;
    height: 30px;
    width: 70px;
    font-size: 16px;
  }

  .link {
    text-decoration: none;
    color: black;
  }
</style>
