<template>
  <LoginLayout>
    <div>
    <h2 align="center">EEvents!
    </h2>

      <g-image src="~/LoginLogo.jpg" width="750"/>
    <v-row justify="center"
           align="center"
    >
      <v-col>
    <v-card
        ref="form"
        width="750"
    >
      <v-card-title class="font-weight-bold">
        Login Form
      </v-card-title>
      <v-card-text>
        <v-text-field
            ref="email"
            v-model="email"
            :rules="[() => !!email || 'This field is required']"
            :error-messages="errorMessages"
            label="E-mail"
            placeholder="example@gmail.com"
            required
            clearable
        ></v-text-field>

        <v-text-field
            ref="password"
            v-model="password"
            :rules="[() => !!password || 'This field is required']"
            :error-messages="errorMessages"
            label="Password"
            required
            :type="'password'"
            clearable
        ></v-text-field>

      </v-card-text>
      <v-divider class="mt-12"></v-divider>
      <v-card-actions>
        <v-spacer></v-spacer>
        <v-slide-x-reverse-transition>
          <v-tooltip
              v-if="formHasErrors"
              left
          >
            <template v-slot:activator="{ on, attrs }">
              <v-btn
                  icon
                  class="my-0"
                  v-bind="attrs"
                  @click="resetForm"
                  v-on="on"
              >
                <v-icon>mdi-refresh</v-icon>
              </v-btn>
            </template>
            <span>Refresh form</span>
          </v-tooltip>
        </v-slide-x-reverse-transition>
        <v-btn
            color="primary"
            text
            @click="login"
        >
          Login
        </v-btn>
        <v-btn
            color="primary"
            text
            @click="signUp"
        >
          SignUp
        </v-btn>
      </v-card-actions>
      <v-alert
          border="left"
          colored-border
          dismissible
          type="error"
          elevation="2"
          transition="scale-transition"
          :value="showAlert"
      >
       Invalid Credentials!
      </v-alert>
    </v-card>
    </v-col>
    </v-row>
    </div>
  </LoginLayout>
</template>

<script>
import LoginLayout from '~/layouts/LoginLayout.vue'
const axios = require("axios");

export default {
  components: {
    LoginLayout
  },
  name: "LoginPage",
  data() {
    return {
      errorMessages: '',
      email: null,
      password: null,
      formHasErrors: false,
      showAlert: false
    };
  },
  computed: {
    form () {
      return {
        email: this.email,
        password: this.password
      }
    },
  },

  watch: {
    name () {
      this.errorMessages = ''
    },
  },
  methods: {
    // login() {
    //   window.axios.post('http://localhost:8082/auth/local', {identifier: this.email, password: this.password})
    //       .then(response => {
    //
    //
    //       })
    //       .catch(error => {
    //         console.log(error);
    //         this.commit('Invalid Credentials')
    //       });
    // }
    resetForm () {
      this.errorMessages = []
      this.formHasErrors = false

      Object.keys(this.form).forEach(f => {
        this.$refs[f].reset()
      })
    },
    login () {
         axios.post('http://localhost:8082/auth/local', {identifier: this.email, password: this.password})
             .then(response => {
               if(response.data.jwt != null){
                 this.$router.push('/Home')
                 this.showAlert = false;
               }
               else{
                 this.showAlert = true;
               }
             })
             .catch(error => {
               console.log(error);
             });
     },
    signUp() {
      this.$router.push('/Signup')
    }
  }
}
</script>

<style>
a {
  text-decoration: none;
  color: black;
}
</style>