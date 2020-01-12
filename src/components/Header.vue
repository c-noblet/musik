<template>
  <nav class="navbar navbar-dark bg-dark mb-3">
    <h1 class="navbar-brand">Musik</h1>
    <div class="input-group w-50">
      <input class="form-control" type="search" placeholder="Search" aria-label="Search" v-model="filtre">
      <div class="input-group-append">
        <button class="btn btn-success" type="submit" v-on:click="searchSongs(filtre)">Search</button>
      </div>
    </div>
    <div v-if="user.isLog == true">
      <div class="dropdown">
        <button class="btn btn-primary btn-sm dropdown-toggle" type="button" id="dropdownMenuButton" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
          {{user.username}}
        </button>
        <div class="dropdown-menu dropdown-menu-right" aria-labelledby="dropdownMenuButton">
          <button class="dropdown-item" data-toggle="modal" data-target="#addModal">Ajouter une musique</button>
          <button v-if="editMode" class="dropdown-item" v-on:click="handleEditMode()">Quitter le mode édition</button>
          <button v-else class="dropdown-item" v-on:click="handleEditMode()">Editer les musiques</button>
          <button v-if="user.isAdmin === true" class="dropdown-item" v-on:click="loadUsersModal()" data-toggle="modal" data-target="#usersModal">Gérer les utilisateurs</button>
          <div class="dropdown-divider"></div>
          <button class="dropdown-item" v-on:click="disconnectUser()">Se déconnecter</button>
        </div>
      </div>
       <div class="modal fade" id="usersModal" tabindex="-1" role="dialog" aria-labelledby="exampleModalLabel" aria-hidden="true">
        <div class="modal-dialog" role="document">
          <div class="modal-content">
            <div class="modal-header">
              <h5 class="modal-title">Gérer les utilisateurs</h5>
              <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                <span aria-hidden="true">&times;</span>
              </button>
            </div>
            <div v-if="!modalLoaded" class="modal-body">
              <div class="text-center">
                <div class="spinner-border text-success" role="status">
                  <span class="sr-only">Loading...</span>
                </div>
              </div>
            </div>
            <div v-if="modalLoaded" class="modal-body">
              <ul class="list-group list-group-flush">
                <li class="list-group-item inline-form" v-for="(user, index) in users" v-bind:key="user.id">
                  <span>
                    {{user.username}} <span v-if="user.admin == 1">is admin</span>
                  </span>
                    <button v-on:click="deleteUser(user.id, index)" class="float-right btn btn-danger btn-sm col-3">Supprimer</button>
                    <button v-on:click="editModal(user.id, user.username, user.prenom, user.nom, user.email)" data-toggle="modal" data-target="#editUserModal" class="float-right btn btn-secondary btn-sm col-3">Modifier</button>
                </li>
              </ul>
            </div>
          </div>
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
                  <input type="file" ref="myPic" class="form-control-file" id="songPic">
                </div>
                <div class="form-group">
                  <label for="songFile">Fichier mp3 :</label>
                  <input type="file" ref="myMp3" class="form-control-file" id="songFile">
                </div>
              </div>
              <div class="modal-footer">
                <button type="button" class="btn btn-primary" data-dismiss="modal" v-on:click="addSong()">Ajouter</button>
              </div>
            </form>
          </div>
        </div>
      </div>
    </div>
    <div v-else>
      <button type="button" class="btn btn-outline-primary" data-toggle="modal" data-target="#loginModal">Se connecter</button>
      <div class="modal fade" id="loginModal" tabindex="-1" role="dialog" aria-labelledby="loginModalLabel" aria-hidden="true">
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
                  <input type="text" class="form-control" v-model="user.username" id="username"/>
                </div>
                <div class="form-group">
                  <label for="password" class="col-form-label">Mot de passe :</label>
                  <input type="password" class="form-control" v-model="user.password" id="password" />
                </div>
              </div>
              <div class="modal-footer">
                <button type="button" class="btn btn-secondary" data-dismiss="modal" data-toggle="modal" data-target="#registerModal">Créer un compte</button>
                <button type="button" class="btn btn-primary" data-dismiss="modal" v-on:click="loginUser(user.username, user.password )">S'authentifier</button>
              </div>
            </form>
          </div>
        </div>
      </div>
      <div class="modal fade" id="registerModal" tabindex="-1" role="dialog" aria-labelledby="registerModalLabel" aria-hidden="true">
        <div class="modal-dialog" role="document">
          <div class="modal-content">
            <div class="modal-header">
              <h5 class="modal-title">Créer un compte</h5>
              <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                <span aria-hidden="true">&times;</span>
              </button>
            </div>
            <form>
              <div class="modal-body">
                <div class="form-group">
                  <label for="username" class="col-form-label">Nom d'utilisateur :</label>
                  <input type="text" class="form-control" v-model="user.username"/>
                </div>
                <div class="form-group">
                  <label for="password" class="col-form-label">Mot de passe :</label>
                  <input type="password" class="form-control" v-model="user.password"/>
                </div>
                <div class="form-group">
                  <label for="username" class="col-form-label">Prenom :</label>
                  <input type="text" class="form-control" v-model="user.prenom"/>
                </div>
                <div class="form-group">
                  <label for="username" class="col-form-label">Nom :</label>
                  <input type="text" class="form-control" v-model="user.nom"/>
                </div>
                <div class="form-group">
                  <label for="username" class="col-form-label">Email :</label>
                  <input type="email" class="form-control" v-model="user.email"/>
                </div>
              </div>
              <div class="modal-footer">
                <button type="button" class="btn btn-secondary" data-dismiss="modal" v-on:click="registerUser(user.username, user.password)">Créer un compte</button>
              </div>
            </form>
          </div>
        </div>
      </div>
    </div>
    <div class="modal fade" id="editUserModal" tabindex="-1" role="dialog" aria-labelledby="editUserModalLabel" aria-hidden="true">
      <div class="modal-dialog" role="document">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title">Mettre à jour l'utilisateur</h5>
            <button type="button" class="close" data-dismiss="modal" aria-label="Close">
              <span aria-hidden="true">&times;</span>
            </button>
          </div>
          <form>
            <div class="modal-body">   
              <div class="form-group">
                <label for="recipient-name" class="col-form-label">Prenom:</label>
                <input type="text" class="form-control" v-model="userEdit.prenom">
              </div>
              <div class="form-group">
                <label for="recipient-name" class="col-form-label">Nom:</label>
                <input type="text" class="form-control" v-model="userEdit.nom">
              </div>
              <div class="form-group">
                <label for="recipient-name" class="col-form-label">email:</label>
                <input type="email" class="form-control" v-model="userEdit.email">
              </div>
              <div class="form-group">
                <label for="recipient-name" class="col-form-label">mot de passe:</label>
                <input type="text" class="form-control" v-model="userEdit.password">
              </div>
            </div>
            <div class="modal-footer">
              <button v-on:click="editUser(userEdit.id)" type="button" class="btn btn-primary" data-dismiss="modal">Sauvegarder</button>
            </div>
          </form>
        </div>
      </div>
    </div>
  </nav>
</template>

<script>

export default {
  props: {
    searchSongs: Function,
    handleEditMode: Function,
    editMode: Boolean,
    forceRender: Function,
    user: Object,
    readCookie: Function,
    writeCookie: Function
  },
  data () {
    return {
      isAdmin: true,
      filtre:'',
      password: '',
      songTitle:'',
      songArtist:'',
      songAlbum:'',
      songAnnee: '',
      songGenre:'',
      users:[],
      mp3:{},
      pic: {},
      userEdit:{
        id: '',
        username: '',
        password: '',
        prenom: '',
        nom: '',
        email: '',
      },
      modalLoaded: false
    }
  },
  methods: {
    addSong: function (){
      const formData = new FormData();
      let pic = document.querySelector('#songPic')
      formData.append('pic', pic.files[0])
      let songFile = document.querySelector('#songFile')
      formData.append('song', songFile.files[0])
      fetch("http://localhost:8000/api_musique/musiques/ajouter/"+this.songTitle+"/"+this.songArtist+"/"+this.songAlbum+"/"+this.songAnnee+"/"+this.songGenre+"", {
        method: 'POST',
        body: formData,
      })
      .then((results) => results.json())
      .then((json) => {
        if(typeof json.id == 'number'){
          this.forceRender()
        }
      }).catch(function(){
        alert('Une erreur est survenue')
      })
    },
    loadUsersModal: function(){
      fetch("http://localhost:8000/user/users")
      .then((results) => results.json())
      .then(json => {
        this.users = json
        this.modalLoaded = true
      }).catch(function(){
        alert('Une erreur est survenue')
      })
      
    },
    registerUser: function(){
      if(this.user.email.indexOf('@') >= 0){
        if(this.user.email.indexOf('.') >= 0){
          this.user.email = this.user.email.split('.').join('-');
          console.log(this.user.email)
        }
        const formData = new FormData();
        fetch("http://localhost:8000/user/ajout/"+this.user.username+"/"+this.user.prenom+"/"+this.user.nom+"/"+this.user.password+"/"+this.user.email, {
          method: 'POST',
          body: formData,
        })
        .then((results) => results.json())
        .then(json => {
          alert('Le compte: '+json.username+' à été crée')
        }).catch(function(){
          alert('Une erreur est survenue')
        })
      }else{
        alert('Email invalide')
      }
    },
    loginUser: function(){
      fetch("http://localhost:8000/user/connexion/"+this.user.username+"/"+this.user.password)
      .then((results) => results.json())
      .then(json => {
        if(json.verif){
          this.writeCookie('user', json.username, 3)
          this.user.id = json.id
          this.user.username = json.username
          this.user.prenom = json.prenom
          this.user.nom = json.nom
          this.user.email = json.email
          this.user.isLog = true
          if(json.admin == 1){
            this.user.isAdmin = true
            this.writeCookie('isAdmin', true, 3)
          }
        }else{
          alert('Identifiant incorrect')
        }
      }).catch(function(){
        alert('Une erreur est survenue')
      })
    },
    disconnectUser: function(){
        this.writeCookie('user', '', 0)
        this.writeCookie('isAdmin', false, 0)
        this.user.id = ''
        this.user.username = ''
        this.user.password = ''
        this.user.prenom = ''
        this.user.nom = ''
        this.user.email = ''
        this.user.isLog = false
        this.user.isAdmin = false
        this.editMode = false
    },
    deleteUser: function(id,index){
      fetch("http://localhost:8000/user/suppression/"+id, {
      method: 'DELETE'
      })
      .then(
        this.users.splice(index, 1)
      ).catch(function(){
        alert('Une erreur est survenue')
      })
    },
    editModal: function(id, username, prenom, nom, email){
      this.userEdit.id = id
      this.userEdit.username = username
      this.userEdit.prenom = prenom
      this.userEdit.nom = nom
      this.userEdit.email = email
    },
    editUser : function(id){
      const formData = new FormData();
      fetch("http://localhost:8000/user/update/"+id+'/'+this.userEdit.username+'/'+this.userEdit.nom+'/'+this.userEdit.prenom+'/'+this.userEdit.password+'/'+this.userEdit.email, {
        method: 'PUT',
        body: formData
      })
      .then(
        alert('Changement réussi')
      ).catch(function(){
        alert('Une erreur est survenue')
      })  
    }
  }
}
</script>