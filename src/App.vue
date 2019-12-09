<template>
  <div id="app">
    <Header
      :searchSongs="searchSongs"
      :handleUser="handleUser"
      :editMode="editMode"
      :handleEditMode="handleEditMode"
      :forceRender="forceRender"
    />
    <Dashboard v-if="dashboardLoaded"
      :editMode="editMode"
      :songs="playlist"
      :skipTo="skipTo"
      :forceRender="forceRender"
    />
    <footer class="fixed-bottom">
      <aplayer v-if="playerLoaded"
        controls
        :list="currentPlaylist"
        :music="currentPlaylist[arrayId]"
      />
    </footer>
    <div v-if="!dashboardLoaded && !playerLoaded" class="text-center mt-5 pt-5">
      <div class="spinner-border text-success" style="width: 10rem; height: 10rem;" role="status">
        <span class="sr-only">Loading...</span>
      </div>
    </div>
  </div>
</template>

<script>
import Aplayer from 'vue-aplayer'
import Header from './components/Header'
import Dashboard from './components/Dashboard'

export default {
  name: 'app',
  data () {
    return {
      arrayId:'0',
      playlist: [],
      currentPlaylist: [],
      dashboardLoaded: false,
      playerLoaded: false,
      editMode: false,
      user:{
        username: null,
        password: null,
        isAdmin: null
      },
      errors: []
    }
  },
  mounted: function(){
    fetch("http://localhost:8000/api_musique/musiques")
    .then((results) => results.json())
    .then(json => {
      this.playlist = json
      this.currentPlaylist = json
      this.dashboardLoaded = true
      this.playerLoaded = true
    });
  },
  methods: {
    handleEditMode: function(){
      this.editMode = !this.editMode
    },
    skipTo: function (e){
      this.arrayId = e-1
    },
    searchSongs: function (filtre){
      this.dashboardLoaded = false
      let list = []
      fetch("http://localhost:8000/api_musique/musiques")
      .then((results) => results.json())
      .then(json => {
        if(filtre !== ''){
          for(let i = 0; i < json.length; i++){
            let song = [json[i].title, json[i].album, json[i].artist, json[i].annee, json[i].genre]
            for(let j = 0; j < song.length; j++){
              if(song[j].toString().toLowerCase().search(filtre.toLowerCase()) != -1){
                list.push(json[i])
                break;
              }
            }
          }
          if(list.length >= 1){
            this.playlist = list
            this.dashboardLoaded = true
          }else{
            this.playlist = json
            this.dashboardLoaded = true
          }  
        }else{
          this.playlist = json
          this.dashboardLoaded = true
        }
        
      });
    },
    handleUser: function (formCase, username, password = ''){
      if(formCase == 'register'){
        fetch("https://jsonplaceholder.typicode.com/users/"+username+"/"+password)
        .then((results) => results.json())
        .then(json => {
          if(json == password){
            console.log('register')
          }
        })
      }else if(formCase == 'login'){
        fetch("https://jsonplaceholder.typicode.com/users/"+username+"/"+password)
        .then((results) => results.json())
        .then(json => {
          if(json == password){
            console.log('login')
          }
        })
      }else if(formCase == 'disconnect'){
        console.log('disconnect')
      }
      this.user.username = username;
      this.user.password = password;
    },
    forceRender : function () {
      fetch("http://localhost:8000/api_musique/musiques")
      .then((results) => results.json())
      .then(json => {
        this.playlist = json
        this.currentPlaylist = json
        this.dashboardLoaded = true
        this.playerLoaded = true
      });
    }
  },
  components: {
    Aplayer, Header, Dashboard
  }
}
</script>

<style lang="css">
  .aplayer, footer{
    background-color: #343a40 !important;
    color: white !important;
  }
  .aplayer-info{
    border: 0 !important;
  }
  .aplayer-list{
    display: none !important;
  }
</style>
