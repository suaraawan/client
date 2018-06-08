<template>
  <div class="register" style="background-color:transparent">
    <div class="container" style="padding-left:5%;">
      <div class="row">
        <div class="col s10" style="margin:4.4%;">
          <form class="col s12">
            <h4 style="text-align:center"><b>Sign Up</b> </h4>
            <div class="row">
              <div class="input-field col s12" style="padding-right:15px;">
                <input id="fullname" type="text" v-model="fullname" v-validate="'required'" name="fullname">
                <label for="fullname">Full Name</label>
                <span class="helper-text">{{ errors.first('fullname') }}</span>
              </div>
              <div class="input-field col s12" style="padding-right:15px;">
                <input id="email" type="email" v-model="email" v-validate="'required|email'" name="email">
                <label for="email">Email</label>
                <span class="helper-text">{{ errors.first('email') }}</span>
              </div>
              <div class="input-field col s12" style="padding-right:15px;">
                <input id="password" v-model="password" v-validate="'required|min:6'" type="password" name="password">
                <label for="password">Password</label>
                <span class="helper-text">{{ errors.first('password') }}</span>
              </div>
              <div class="input-field col s12" style="padding-right:15px;">
                <input id="confirmpassword" type="text" class="validate">
                <label for="confirmpassword">Confirm Password</label>
              </div>
            </div>
            <a class="btn btn-block" @click='signUp'>Sign Up</a>
            <br>
            <p>Already have an account? <a @click='backToHomepage' style="cursor: pointer;"><u>Sign In here</u></a> </p>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'register',
  created () {
    if (localStorage.hasOwnProperty('authorization')) {
      // this.$router.push('/todo')
    } else {
      (function (d, s, id) {
        var js
        var fjs = d.getElementsByTagName(s)[0]
        if (d.getElementById(id)) return
        js = d.createElement(s)
        js.id = id
        js.src = '//connect.facebook.net/en_US/sdk.js'
        fjs.parentNode.insertBefore(js, fjs)
      }(document, 'script', 'facebook-jssdk'))
      window.fbAsyncInit = function () {
        window.FB.init({
          appId: '223165648453545',
          cookie: true,
          xfbml: true,
          version: 'v2.8'
        })
      }
    }
  },
  data () {
    return {
      fullname: '',
      email: '',
      password: ''
    }
  },
  methods: {
    signUp (event) {
      event.preventDefault()
      const newUser = {
        fullname: this.fullname,
        email: this.email,
        password: this.password
      }
      axios({
        method: 'post',
        url: 'http://localhost:3000/users/signup',
        data: newUser
      }).then(response => {
        console.log('success register');
      }).catch(err => {
        console.log(err);
      })
    },
    backToHomepage () {
      this.$router.push('/')
    }
  }
}
</script>

<style scoped>
.register{
  background-color: #ecf0f1;
  background-size: cover;
  padding-top: 10px;
  height: 100%;
}
.helper-text {
  color: red;
}
</style>
