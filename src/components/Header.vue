<template>
  <nav class="navbar navbar-dark bg-dark mb-3">
    <h1 class="navbar-brand">Musik</h1>
    <div class="input-group w-50">
      <input class="form-control" type="search" placeholder="Search" aria-label="Search" v-model="filtre">
      <div class="input-group-append">
        <button class="btn btn-success" type="submit" v-on:click="searchSongs(filtre)">Search</button>
      </div>
    </div>
    <form class="form-inline">
      <label for="login"></label>
      <button type="button" class="btn btn-outline-primary" data-toggle="modal" data-target="#loginModal">Se connecter</button>
    </form>
    <div class="modal fade" id="loginModal" tabindex="-1" role="dialog" aria-labelledby="exampleModalLabel" aria-hidden="true">
      <div class="modal-dialog" role="document">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title" id="exampleModalLabel">Se connecter</h5>
            <button type="button" class="close" data-dismiss="modal" aria-label="Close">
              <span aria-hidden="true">&times;</span>
            </button>
          </div>
          <div class="modal-body">
            <form>
              <div class="form-group">
                <label for="login" class="col-form-label">Nom d'utilisateur :</label>
                <input type="text" class="form-control" id="login" />
              </div>
              <div class="form-group">
              <label for="password" class="col-form-label">Mot de passe :</label>
                <input type="password" class="form-control" id="password" />
              </div>
            </form>
          </div>
          <div class="modal-footer">
          <button type="button" class="btn btn-secondary">Créer un compte</button>
            <button type="button" class="btn btn-primary">S'authentifier</button>
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
  },
  data () {
    return {
      filtre:''
    }
  }
}
</script>