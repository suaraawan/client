<template>
  <div class="hello"  style="height:100%;">
    <nav>
      <div class="nav-wrapper">
        <a class="brand-logo left" style="cursor: default;"><i class="fab fa-soundcloud" style="color:#ff4900"></i><b>Suara Awan</b></a>
        <ul id="nav-mobile" class="right hide-on-med-and-down">
          <li style="margin-right:25px;" @click='goToRegister' v-if='checkStorage() == false'><a>Register</a></li>
          <li class="modal-trigger" data-target="modal1" style="margin-right:25px;" @click='' v-if='checkStorage() == true'><a>Add New Todo</a></li>
          <li><a v-if='checkStorage() == true'>Today <span class="new badge">5</span></a></li>
          <li style="margin-right:25px;" @click='' v-if='checkStorage() == true'><a>History</a></li>
          <li style="margin-right:25px;" @click='' v-if='checkStorage() == true' @click=''><a>Logout</a></li>
        </ul>
      </div>
    </nav>

  </div>
</template>

<script>

export default {
  created () {
    this.checkStorage()
  },
  data () {
    return {

    }
  },
  methods: {
    goToRegister () {
      this.$router.push('register')
    },
    checkStorage () {
      if (!localStorage.hasOwnProperty('authorization')) {
        return false
      } else {
        return true
      }
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
    padding-left: 15px;
}

.modal {
width: 40%;
height: 100%;
margin-top: 0px;
padding: 0 0 0 0;
}

h5
{
font-weight: 400;
}

.container{
margin: 0 0 0 0;
width: 100%;
height: 100%;
}

.tabs .indicator
{
background-color: #1e2121;
opacity: 0.3;
}

.form-container
{
padding: 40px;
padding-top: 10px;
}

.confirmation-tabs-btn
{
position: absolute;
}
</style>
