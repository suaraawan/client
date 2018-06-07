<template>
  <div class="hello"  style="height:100%;">

    <div class="row">
      <div class="col s4" style="margin:3.3% 0 0 60%;">
        <form class="col s12">
          <h4 style="text-align:left"> <b>Sign In</b> </h4>
          <div class="row">
            <div class="input-field col s12" style="padding-right:15px;">
              <input id="email" type="email" class="validate" v-model='email'>
              <label for="email">Email</label>
            </div>
            <div class="input-field col s12" style="padding-right:15px;">
              <input id="password" type="password" class="validate" v-model='password'>
              <label for="password">Password</label>
            </div>
          </div>
          <a class="btn btn-block" style="width:100%; color:white" @click='login'>Sign In</a>
          <p>or Sign In with :</p>
          <a class="btn btn-block socmed" style="background-color:#3867d6; font-size:15px;" @click="loginfb">Sign In with facebook</a>
        </form>
      </div>
    </div>

  </div>
</template>

<script>

export default {
  name: 'Homepage',
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
      email: '',
      password: '',
      googleSignInParams: {
        client_id: '108913565945-7db24779940ae01k701aniiue3cmj5se.apps.googleusercontent.com'
      }
    }
  },
  methods: {
    login () {
      // manual login
      const UserData = {
        email: this.email,
        password: this.password
      }
      // axios({
      //   method: 'post',
      //   url: 'http://localhost:3000/users/login',
      //   data: UserData
      // }).then((response) => {
      //   const token = JSON.stringify(response.data.jwtToken)
      //   localStorage.setItem('authorization', token);
      //   this.$router.push('/todo')
      //   this.email = ''
      //   this.password = ''
      // }).catch((err) => {
      //   console.log(err)
      // })
      console.log(UserData);
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
          console.log(response);
        }
      })
    }
  }
}

</script>

<style scoped>
ul {
  list-style-type: none;
  padding: 0;
}
a {
  color: #dfe6e9;
}

nav {
  -webkit-transform: none;
  transform: none;
  text-align: left;
  background-color: #2d3436;
  font-family: sans-serif;
  text-align: center;
}
.navmedia{
  padding: 0 5 0 0;
  background-color: transparent;
  border: none;
  margin-bottom: 13%;
}
.socmed{
  height:39px;
  width: 100%;
  text-align: center;
  font-size: 50px;
  margin:0 0 27.5% 0;
}
li{
  margin-right:10px;
}
.tooltip {
    position: relative;
    display: inline-block;
    border-bottom: 1px dotted black;
}

.tooltip .tooltiptext {
    visibility: hidden;
    width: 120px;
    background-color: #555;
    color: #fff;
    text-align: center;
    border-radius: 6px;
    padding: 5px 0;
    position: absolute;
    z-index: 1;
    bottom: 125%;
    left: 50%;
    margin-left: -60px;
    opacity: 0;
    transition: opacity 0.3s;
    font-size: 12px;
}

.tooltip .tooltiptext::after {
    content: "";
    position: absolute;
    top: 100%;
    left: 50%;
    margin-left: -5px;
    border-width: 5px;
    border-style: solid;
    border-color: #555 transparent transparent transparent;
}

.tooltip:hover .tooltiptext {
    visibility: visible;
    opacity: 1;
}
</style>
