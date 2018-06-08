<template>
  <div class="header"  style="height:100%; width:100%;">
    <nav>
      <div class="nav-wrapper">
        <a class="brand-logo left" style="cursor: default;cursor:pointer;" @click='backToHome'><i class="fab fa-soundcloud" style="color:#ff4900;" ></i><b>Suara Awan</b></a>
        <ul id="nav-mobile" class="right hide-on-med-and-down">
          <li style="margin-right:25px;" @click='goToUser' v-if=''><a>Profile</a></li>
          <li class="modal-trigger" data-target="modal1" style="margin-right:25px;" v-if=''><a>Add New Song</a></li>
          <li style="margin-right:25px;" @click='' v-if='checkStorage() == true' @click='logout'><a>Logout</a></li>
        </ul>
      </div>
    </nav>
    <!-- Modal -->
    <div id="modal1" class="modal">
      <div class="container">
      	<ul class="tabs teal">
      		<li class="tab" style="width:100%;"><b>Add new Song</b></li>
      	</ul>
        <br>
      	<div id="login" class="col s12">
      		<form class="col s12">
      			<div class="form-container">
              <div class="input-field col s12">
                <input id="title" type="text" v-model='title'>
                <label for="title">Title</label>
              </div>
              <div class="input-field col s12">
                <input id="artist" type="text" v-model='artist'>
                <label for="artist">Artist</label>
              </div>
              <div class="file-field input-field">
                <div class="btn">
                  <span>File</span>
                  <input type="file" @change="filename">
                </div>
                <div class="file-path-wrapper">
                  <input class="file-path validate" type="text">
                </div>
              </div>
      				<br>
      				<center>
                <a class="btn modal-action modal-close waves-effect waves-light teal" style="margin-right:5px;" @click='reset'>Done</a>
      					<button class="btn waves-effect waves-light teal" @click='addNewSong'>Upload</button>
      					<br>
      				</center>
      			</div>
      		</form>
      	</div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  created () {
    this.checkStorage()
  },
  data () {
    return {
      title: '',
      artist: '',
      file: null,
      songs: []
    }
  },
  methods: {
    checkStorage () {
      if (!localStorage.hasOwnProperty('authorization')) {
        return false
      } else {
        return true
      }
    },
    goToUser () {
      this.$router.push('user')
    },
    logout () {
      localStorage.clear();
      this.$router.push('/')
    },
    backToHome () {
      this.$router.push('music')
    },
    addNewSong () {
      let formData = new FormData()
      console.log(this.file);
      formData.append('url', this.file)
      formData.append('artist', this.artist)
      formData.append('title', this.title)

      const auth = localStorage.getItem('authorization')

      axios({
        method: 'post',
        url: 'http://localhost:3000/musics/upload',
        data: formData,
        headers : { token: auth }
      }).then(response => {
        swal({
          title: "Yosh!",
          text: "Successfully add new song",
          icon: "success",
        });
        this.getSong()
      }).catch(err => {
        console.log(err);
      })
    },
    filename (event) {
      this.file = event.target.files[0]
    },
    reset () {
      this.title = ''
      this.artist = ''
      this.file = null
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
