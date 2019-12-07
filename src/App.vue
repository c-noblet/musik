<template>
  <div id="app">
    <Header 
      :searchSongs="searchSongs"
    />
    <Dashboard 
      :songs="playlist"
      :skipTo="skipTo"

    />
    <footer class="fixed-bottom">
      <aplayer
        :music="{
          title: titre,
          artist: artist,
          list: playlist,
          src: 'http://localhost:8000/api_musique/musiques/get/musique/'+uid,
          pic: 'http://localhost:8000/api_musique/musiques/get/image/'+uid
        }"
      />
    </footer>
    
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
      uid:'0',
      playlist: [],
      titre: '',
      artist: ''
    }
  },
  mounted: function(){
    fetch("http://localhost:8000/api_musique/musiques")
    .then((results) => results.json())
    .then(json => {
      this.uid = json[0].id,
      this.playlist = json,
      this.titre = json[0].titre
      this.artist = json[0].artiste
    });
  },
  methods: {
    skipTo: function (e){
      this.uid = e
      this.titre = this.playlist[e-1].titre
      this.artist = this.playlist[e-1].artiste

    },
    searchSongs: function (filtre){
      let list = []
      fetch("http://localhost:8000/api_musique/musiques")
      .then((results) => results.json())
      .then(json => {
        if(filtre !== ''){
          for(let i = 0; i < json.length; i++){
            let song = [json[i].titre, json[i].album, json[i].artiste, json[i].annee, json[i].genre]
            for(let j = 0; j < song.length; j++){
              if(song[j].toString().toLowerCase().search(filtre.toLowerCase()) != -1){
                list.push(json[i])
                break;
              }
            }
          }
          console.log(list)
          this.playlist = list
        }else{
          this.playlist = json
        }
        
      });
    }
  },
  components: {
    Aplayer, Header, Dashboard
  }
}
</script>
