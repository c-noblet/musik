<template>
  <div class="container-fluid">
    <ul class="list-group list-group-horizontal d-flex flex-wrap justify-content-around">
      <li class="mx-2 mb-3 card" v-for="song in songs" :key="song.id" style="max-width: 300px" v-on:click="skipTo(song.id)">
        <div class="row no-gutters">
          <div class="col-4">
            <img :src="song.pic" class="card-img" alt="">
          </div>
          <div class="col-8">
            <div class="card-body p-3">
              <h5 class="card-title">{{song.title}}</h5>
              <p class="card-text">
                {{song.artist}}
                <small class="text-muted">{{song.annee}}</small>
              </p>
            </div>
          </div>
        </div>
        <button v-if="editMode" data-toggle="modal" data-target="#editModal" class="btn btn-edit btn-sm btn-outline-primary" v-on:click="openModal(song.id)">Edit</button>
      </li>
    </ul>
    <div class="modal fade" id="editModal" tabindex="-1" role="dialog" aria-labelledby="exampleModalLabel" aria-hidden="true">
      <div class="modal-dialog" role="document">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title">Modifications</h5>
            <button type="button" class="close" data-dismiss="modal" aria-label="Close">
              <span aria-hidden="true">&times;</span>
            </button>
          </div>
          <div class="modal-body">
            <div class="form-group">
              <label for="songTitle" class="col-form-label">Titre :</label>
              <input type="text" class="form-control" v-model="songTitle" id="songTitle"/>
            </div>
            <div class="form-group">
              <label for="songArtist" class="col-form-label">Artiste :</label>
              <input type="text" class="ml-1 form-control" v-model="songArtist" id="songArtist" />
            </div>
            <div class="form-group">
              <label for="songAlbum" class="col-form-label">Album :</label>
              <input type="text" class="ml-1 form-control" v-model="songAlbum" id="songAlbum" />
            </div>
            <div class="form-group">
              <label for="songAnnee" class="col-form-label">Année :</label>
              <input type="text" class="ml-1 form-control" v-model="songAnnee" id="songAnnee" />
            </div>
            <div class="form-group">
              <label for="songGenre" class="col-form-label">Genre :</label>
              <input type="text" class="ml-1 form-control" v-model="songGenre" id="songGenre" />
            </div>
          </div>
          <div class="modal-footer">
            <button type="button" class="btn btn-danger" v-on:click="deleteSong()">Supprimer</button>
            <button type="button" class="btn btn-primary" v-on:click="editSong()">Sauvegarder</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>

export default {
  props: {
    songs: Array,
    skipTo: Function,
    search: Function,
    editMode: Boolean
  },
  data () {
    return {
      username: '',
      password: '',
      songId: '',
      songTitle:'',
      songArtist:'',
      songAlbum:'',
      songAnnee:'',
      songGenre:''
    }
  },
  methods:{
    openModal: function(id){
      this.songId = id
      this.songTitle = this.songs[id-1].title
      this.songArtist = this.songs[id-1].artist
      this.songAlbum = this.songs[id-1].album
      this.songAnnee = this.songs[id-1].annee
      this.songGenre = this.songs[id-1].genre
    },
    editSong: function(){
      console.log('PUT')
      fetch("http://localhost:8000/api_musique/musiques/modifer/"+this.songId+"/"+this.songTitle+"/"+this.songArtist+"/"+this.songAlbum+"/"+this.songAnnee+"/"+this.songGenre+"", {
        method: 'PUT'
      })
      .then((results) => results.json())
      .then(json => {
        console.log(json)
      })
    },
    deleteSong: function(){
      fetch("http://localhost:8000/api_musique/musiques/delete/"+this.songId, {
        method: 'DELETE'
      })
      .then((results) => results)
      .then(json => {
        console.log(json)
      })
    }
  }
}
</script>

<style lang="css" scoped>
.container-fluid{
  overflow-y: auto;
}
.btn-edit{
  position: absolute;
  top:0;
  right: 0;
}
li{
  list-style: none;
}
.card{
  max-width: 300px;
  height: 100px;
}
.no-gutters, img{
  height: 100%;
}
</style>