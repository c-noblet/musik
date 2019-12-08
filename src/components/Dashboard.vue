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
              <p class="card-text">{{song.artiste}} <small class="text-muted">{{song.annee}}</small></p>
            </div>
          </div>
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  props: {
    songs: Array,
    skipTo: Function,
    search: Function,
  }
}
</script>

<style lang="css" scoped>
.container-fluid{
  overflow-y: auto;
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