<template lang="html">
  <div class="">
    <img src="http://www.oroplatino.eu/wp-content/uploads/2017/04/oroplatino-user-page-background.jpg" alt="" style="width:100%; height:550px;">
    <br><br><br>
    <div class="row">
      <div class="col s12 m12" style="margin-bottom:50px;" v-for='song in songs'>
        <div class="card blue-grey darken-1">
          <a class="btn-floating halfway-fab waves-effect waves-light red" @click='deleteSong(song._id)'><i class="material-icons">delete</i></a>
          <div class="card-content" style="text-align:left; color:white;">
            <h5>Artist : {{ song.artist }}</h5>
            <hr>
            <h5>Title : {{ song.title }}</h5>
          </div>
          <hr>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  created () {
    if (!localStorage.hasOwnProperty('authorization')) {
      this.$router.push('/')
    } else {
      this.getSong()
    }
  },
  data () {
    return {
      songs: null
    }
  },
  methods: {
    getSong () {
      const token = localStorage.getItem('authorization')
      axios({
        method: 'get',
        url: 'http://localhost:3000/musics/get',
        headers : { token: token }
      }).then(response => {
        this.songs = response.data.data
        console.log(this.songs);
      }).catch(err => {
        console.log(err);
      })
    },
    deleteSong (song_id) {
      const self = this
      axios({
        method: 'delete',
        url: `http://localhost:3000/musics/${song_id}`
      }).then(response => {
        self.getSong()
      }).catch(err => {
        console.log(err);
      })
    }
  }
}
</script>

<style scoped>

</style>
