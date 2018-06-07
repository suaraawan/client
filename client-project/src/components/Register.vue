<template>
  <div class="register" style="background-color:transparent">
    <div class="container" style="padding-left:5%;">
      <div class="row">
        <div class="col s10" style="margin:4.4%;">
          <form class="col s12">
            <h4 style="text-align:center"><b>Sign Up</b> </h4>
            <div class="row">
              <div class="input-field col s12" style="padding-right:15px;">
                <input id="fullname" type="text" class="validate">
                <label for="fullname">Full Name</label>
              </div>
              <div class="input-field col s12" style="padding-right:15px;">
                <input id="email" type="text" class="validate">
                <label for="email">Email</label>
              </div>
              <div class="input-field col s12" style="padding-right:15px;">
                <input id="password" type="text" class="validate">
                <label for="password">Password</label>
              </div>
              <div class="input-field col s12" style="padding-right:15px;">
                <input id="confirmpassword" type="text" class="validate">
                <label for="confirmpassword">Confirm Password</label>
              </div>
            </div>
            <a class="btn btn-block">Sign Up</a>
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

    }
  },
  methods: {
    backToHomepage () {
      this.$router.push('/')
    },
    onSignInSuccess (googleUser) {
      // google login
      const profile = googleUser.getBasicProfile()
      const userData = {
        google_id: profile.getId(),
        username: profile.getName(),
        user_image: profile.getImageUrl(),
        email: profile.getEmail()
      }
      axios({
        method: 'post',
        url: 'http://localhost:3000/users/loginGoogle',
        data: userData
      }).then((response) => {
        console.log('login success')
      }).catch((err) => {
        console.log(err)
      })
    },
    onSignInError (error) {
      console.log('OH NOES', error)
    },
    loginfb () {
      // fb login
      window.FB.login((response) => {
        if (response.status === 'connected') {
          localStorage.setItem('fb_access_token', response.authResponse.accessToken)
          // axios({
          //   method: 'post',
          //   url: 'http://localhost:3000/users/loginFb',
          //   headers: { fb_access_token: localStorage.getItem('fb_access_token') }
          // }).then((response) => {
          //   console.log('Welcome!  Fetching your information.... ')
          //   localStorage.setItem('token', response.data.token)
          // }).catch((err) => {
          //   console.log(err)
          // })
        }
      })
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
</style>
