<template>
  <nav class="navbar navbar-dark bg-dark mb-3">
    <h1 class="navbar-brand">Musik</h1>
    <div class="input-group w-50">
      <input class="form-control" type="search" placeholder="Search" aria-label="Search" v-model="filtre">
      <div class="input-group-append">
        <button class="btn btn-success" type="submit" v-on:click="searchSongs(filtre)">Search</button>
      </div>
    </div>
    <div v-if="isLog">
      <div class="dropdown">
        <button class="btn btn-primary btn-sm dropdown-toggle" type="button" id="dropdownMenuButton" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
          Coco
        </button>
        <div class="dropdown-menu dropdown-menu-right" aria-labelledby="dropdownMenuButton">
          <button class="dropdown-item" data-toggle="modal" data-target="#addModal">Ajouter une musique</button>
          <button v-if="editMode" class="dropdown-item" v-on:click="handleEditMode()">Quitter le mode édition</button>
          <button v-else class="dropdown-item" v-on:click="handleEditMode()">Editer les musiques</button>
          <button class="dropdown-item" data-toggle="modal" data-target="#usersModal" v-if="isAdmin">Gérer les utilisateurs</button>
          <div class="dropdown-divider"></div>
          <button class="dropdown-item" v-on:click="handleUser('disconnect', username)">Se déconnecter</button>
        </div>
      </div>
      <div class="modal fade" id="addModal" tabindex="-1" role="dialog" aria-labelledby="exampleModalLabel" aria-hidden="true">
        <div class="modal-dialog" role="document">
          <div class="modal-content">
            <div class="modal-header">
              <h5 class="modal-title">Ajouter une musique</h5>
              <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                <span aria-hidden="true">&times;</span>
              </button>
            </div>
            <form>
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
                <div class="form-group">
                  <label for="songPic">Image :</label>
                  <input type="file" @change="previewPic" ref="myPic" class="form-control-file" id="songPic">
                </div>
                <div class="form-group">
                  <label for="songMp3">Fichier mp3 :</label>
                  <input type="file" @change="previewMp3" ref="myMp3" class="form-control-file" id="songMp3">
                </div>
              </div>
              <div class="modal-footer">
                <button type="button" class="btn btn-primary" v-on:click="addSong()">Ajouter</button>
              </div>
            </form>
          </div>
        </div>
      </div>
    </div>
    <div v-else>
      <button type="button" class="btn btn-outline-primary" data-toggle="modal" data-target="#loginModal">Se connecter</button>
      <div class="modal fade" id="loginModal" tabindex="-1" role="dialog" aria-labelledby="exampleModalLabel" aria-hidden="true">
        <div class="modal-dialog" role="document">
          <div class="modal-content">
            <div class="modal-header">
              <h5 class="modal-title">Se connecter</h5>
              <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                <span aria-hidden="true">&times;</span>
              </button>
            </div>
            <form>
              <div class="modal-body">
                <div class="form-group">
                  <label for="username" class="col-form-label">Nom d'utilisateur :</label>
                  <input type="text" class="form-control" v-model="username" id="username"/>
                </div>
                <div class="form-group">
                  <label for="password" class="col-form-label">Mot de passe :</label>
                  <input type="password" class="form-control" v-model="password" id="password" />
                </div>
              </div>
              <div class="modal-footer">
                <button type="button" class="btn btn-secondary" v-on:click="handleUser('register', username, password)">Créer un compte</button>
                <button type="button" class="btn btn-primary" v-on:click="handleUser('login', username, password )">S'authentifier</button>
              </div>
            </form>
          </div>
        </div>
      </div>
    </div>
  </nav>
</template>

<script>

export default {
  props: {
    searchSongs: Function,
    handleUser: Function,
    handleEditMode: Function,
    editMode: Boolean,
    forceRender: Function
  },
  data () {
    return {
      isLog: true,
      isAdmin: true,
      filtre:'',
      username: '',
      password: '',
      songTitle:'',
      songArtist:'',
      songAlbum:'',
      songAnnee:'',
      songGenre:'',
      mp3:{},
      pic: {}
    }
  },
  methods: {
    previewMp3: function () {
      this.mp3 = this.$refs.myMp3.files
    },
    previewPic: function () {
      this.pic = this.$refs.myPic.files
    },
    addSong: function (){
      const formData = new FormData();
      formData.append('mp3', this.mp3)
      formData.append('pic', this.pic)
      fetch("http://localhost:8000/api_musique/musiques/ajouter/test/test/test/2020/test", {
        method: 'POST',
        body: formData
      })
      .then((results) => results)
      .then(json => {
        console.log(json)
      })
    }
  }
}
</script>