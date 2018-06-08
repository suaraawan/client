<template lang="html">
  <div class="music">
    <img src="https://www.wallpaperup.com/uploads/wallpapers/2013/05/07/83351/6492d576032a11b62c06582cf9ef926a-700.jpg" alt="" style="width:100%; height:550px;">
    <div class="row">
      <div class="col s6 m4" style="text-align:center;" v-for='song in songs'>
        <div class="card">
          <div class="card-image">
            <img src="http://graphiccave.com/wp-content/uploads/2015/04/Music-Icon-JPG-Graphic-Cave-1080x565.jpg" style="width:100%; height:100%;">
          </div>
          <div class="card-content">
            <h5>Artist : {{song.artist}}</h5>
            <h5>Title : {{song.title}}</h5>
          </div>
          <div class="card-action">
            <audio controls>
              <source :src="song.url" type="audio/ogg">
            </audio>
          </div>
          <div class="card-action black">
            <a class="waves-effect waves-light modal-trigger" data-target="modal1" @click='getApi(song.artist, song.title, song.artist, song.title)'> <i class="fas fa-align-center"></i> |  Get This Song Lyric</a>
          </div>
          <div class="card-action blue darken-3">
            <a class="waves-effect" @click='shareFb(song.url, song.artist, song.title)' style="color:white"><i class="fab fa-facebook-square"></i> | SHARE WITH FACEBOOK </a>
          </div>
        </div>
      </div>
    </div>
    <footer class="page-footer" style="background-color:black">
      <div class="footer-copyright">
        <div class="container">
          <button type="button" name="button" class="btn btn-blocked btn-large" style="width:100%;" v-if='lyric != null' @click='removeLyric'>Close Lyric</button>
          <br v-if='lyric != null'><br v-if='lyric != null'><hr v-if='lyric != null'><br v-if='lyric != null'>
          <h3 v-if='lyric != null'>Artist : {{ artistLyric }}</h3>
          <h4 v-if='lyric != null'>Title : {{ titleLyric }}</h4>
          <br v-if='lyric != null'><br v-if='lyric != null'><hr v-if='lyric != null'><br v-if='lyric != null'>
          <p v-if='lyric == null'>Get Your Lyric's Here !</p>
          <br>
          <pre>{{ lyric }}</pre>
        </div>
      </div>
    </footer>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  created () {
    if (!localStorage.hasOwnProperty('authorization')) {
      this.$router.push('/')
    } else {
      this.getSongs()
    }
  },
  data () {
    return {
      url: '',
      title: '',
      userId: '',
      artist: '',
      lyric: null,
      songs: null,
      openLyric: true,
      artistLyric: '',
      titleLyric: ''
    }
  },
  methods: {
    getSongs () {
      let self = this
      axios({
        method: 'get',
        url: 'http://localhost:3000/musics/list'
      }).then(response => {
        self.songs = response.data.musics
      }).created(err => {
        console.log(err);
      })
    },
    getApi (artist, title, songartist, songlyric) {
      artist = artist.toLowerCase()
      title = title.toLowerCase()
      let self = this
      let splitartist = ''
      let splittitle = ''
      for (var i = 0; i < artist.length; i++) {
        if (artist[i] == ' ') {
          splitartist += '%20'
        } else {
          splitartist += artist[i]
        }
      }
      for (var i = 0; i < title.length; i++) {
        if (title[i] == ' ') {
          splittitle += '%20'
        } else {
          splittitle += title[i]
        }
      }

      axios({
        method: 'get',
        url: `https://api.lyrics.ovh/v1/${splitartist}/${splittitle}`
      }).then(response => {
        self.lyric = response.data.lyrics
        self.artistLyric = songartist
        self.titleLyric = songlyric
      }).catch(err => {
        console.log(err);
      })

    },
    openLyricSong () {
      this.openLyric = true
    },
    removeLyric () {
      this.lyric = null
    },
    shareFb (urlsong, artist, title) {
      FB.ui({
        method: 'share',
        display: 'popup',
        href: `${urlsong}`,
        quote: `
        Artist : ${artist}
        Title : ${title}

        ${urlsong}
        `,
      }, function(response){});
    }
  }
}
</script>

<style scoped>
.music{
  background-color: #eceff1;
}
.modal {
width: 80%;
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
/* position: */
}

.confirmation-tabs-btn
{
position: absolute;
}
</style>
