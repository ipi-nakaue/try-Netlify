<template>
  <div class="loginbackgroud">
    <b-row class="text-center" align-v="center">
      <b-col cols="1" md="4" />
      <b-col cols="10" sm="10" md="4">
        <h1 style="margin-top: 60px;">Create Account</h1>
      </b-col>
    </b-row>
    <b-container fluid>
      <b-row class="text-center" align-v="center">
        <b-col cols="1" md="5" />
        <b-col cols="10" sm="10" md="2">
          <b-form style="margin-top: 30px;" @submit="signUp">
            <b-form-group label="Email address" label-for="input-1">
              <b-form-input
                id="input-1"
                type="email"
                required
                placeholder="Enter email"
                v-model="email"
              ></b-form-input>
            </b-form-group>
            <b-form-group label="Password" label-for="input-2">
              <b-form-input
                id="input-2"
                type="password"
                required
                placeholder="Enter password"
                v-model="password"
              ></b-form-input>
            </b-form-group>
            <b-form-group label="ConfirmPassword" label-for="input-2">
              <b-form-input
                id="input-2"
                type="password"
                required
                placeholder="Enter confirm password"
                v-model="confirmPassword"
              ></b-form-input>
            </b-form-group>
            <b-form-group style="margin-top: 30px;">
              <b-button block type="submit" variant="success">
                Sign Up
              </b-button>
            </b-form-group>
          </b-form>
        </b-col>
        <b-col cols="1" />
      </b-row>
    </b-container>
  </div>
</template>

<script>
import firebase from 'firebase'
import { mapActions } from 'vuex'

export default {
  data() {
    return {
      email: '',
      password: '',
      confirmPassword: ''
    }
  },
  methods: {
    signUp(event) {
      event.preventDefault()
      console.log('sign up.')

        event.preventDefault()
        if (this.password !== this.confirmPassword) {
            alert('Not match confirm password.')
            return
        }
        const user = {
            email: this.email,
            password: this.password
        }
        this.createUser(user)
            .then(() => {
            this.$router.push('/')
            })
            .catch((error) => {
            alert(error)
        })
    },
    ...mapActions('user', ['createUser'])
  }
}
</script>
