<template>
  <div id="app">
    <div>
      <router-link to="/login">Login</router-link>
      <router-view></router-view>

      <div class="sign-up-container">
        <form
          class="column left"
          @submit.prevent="submitForm"
          method="post"
          Content-Type="multipart/form-data"
          id="sign-up-form"
        >
          <svg style="width: 96px; height: 96px" viewBox="0 0 24 24">
            <path
              fill="var(--color4)"
              d="M12.5 7C12.5 5.89 13.39 5 14.5 5H18C19.1 5 20 5.9 20 7V9.16C18.84 9.57 18 10.67 18 11.97V14H12.5V7M6 11.96V14H11.5V7C11.5 5.89 10.61 5 9.5 5H6C4.9 5 4 5.9 4 7V9.15C5.16 9.56 6 10.67 6 11.96M20.66 10.03C19.68 10.19 19 11.12 19 12.12V15H5V12C5 10.9 4.11 10 3 10S1 10.9 1 12V17C1 18.1 1.9 19 3 19V21H5V19H19V21H21V19C22.1 19 23 18.1 23 17V12C23 10.79 21.91 9.82 20.66 10.03Z"
            />
          </svg>
          <div class="welcome">
            <h1>Welcome to LivingRoom</h1>
            <p>Here you can post and share with friends!</p>
            <i>Sign up, It's Free!!</i>
          </div>
          <input
            type="text"
            name="username"
            placeholder="Username"
            v-model="user.username"
            class="input"
          />
          <input
            type="text"
            name="email"
            placeholder="Email"
            v-model="user.email"
            class="input"
          />
          <input
            type="password"
            name="password"
            placeholder="Password"
            v-model="user.password"
            class="input"
          />
          <button class="btn" id="btn-signup">Sign Up</button>
        </form>

        <div class="column right">
          <form
            @submit.prevent="ingresar"
            method="post"
            Content-Type="multipart/form-data"
            id="login-form"
          >
            <h4>Log In</h4>
            <input type="text" placeholder="Email" v-model="user.email" />
            <input
              type="password"
              placeholder="Password"
              v-model="user.password"
            />
            <button class="btn btn-login" id="btn-signUp">Log In</button>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Vue from "vue";
import axios from "axios";
import VueAxios from "vue-axios";

Vue.use(VueAxios, axios);
export default {
  name: "App",
  data() {
    return {
      user: {
        username: null,
        email: null,
        password: null,
      },
    };
  },
  methods: {
    submitForm: function (e) {
      let user = {
        un: this.user.username,
        e: this.user.email,
        p: this.user.password,
      };

      axios
        .post("http://localhost:8010/proxy/api/auth/register", {
          username: user.un,
          email: user.e,
          password: user.p,
        })
        .then((response) => {
          console.log(response.data);
          this.$swal("You are registred!! Now try to Login :)");
          document.getElementsByClassName("input").value = "";
        })
        .catch((e) => {
          console.log(e);
          this.$swal("The user already exists! :(");
        });

      e.preventDefault();
    },
    ingresar: function () {
      let user = {
        e: this.user.email,
        p: this.user.password,
      };

      axios
        .post("http://localhost:8010/proxy/api/auth/login", {
          email: user.e,
          password: user.p,
        })
        .then((res) => {
          console.log(res.status);
          this.$swal("You can login :)");
        })
        .catch((e) => {
          console.log(e);
          this.$swal("You can't login :(");
        });
    },
  },
};
</script>

<style>
@import "./assets/css/styles.css";
</style>
