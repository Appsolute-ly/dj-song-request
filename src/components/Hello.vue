<template>
  <div class="container">
  <div class="page-header">
    <h1>{{ msg }}</h1>
  </div>


  <div class="panel panel-default">
    <div class="panel-heading">
      <h3>Add Song</h3>
    </div>
    <div class="panel-body">
      <form id="form" class="form-inline" v-on:submit.prevent="addSong">
        <div class="form-group">
          <label for="songTitle">Title:</label>
          <input type="text" id="songTitle" class="form-control" v-model="newSong.title">
        </div>
        <div class="form-group">
          <label for="songArtist">Artist:</label>
          <input type="text" id="songArtist" class="form-control" v-model="newSong.artist">
        </div>
        <input type="submit" class="btn btn-primary" value="Add Song">
      </form>
    </div>
  </div>
  <div class="panel panel-default">
    <div class="panel-heading">
      <h3>Songs</h3>
    </div>
    <div class="panel-body">
      <table class="table table-striped">
        <thead>
          <tr>
            <th>
              Title
            </th>
            <th>
              Artist
            </th>
            <th>
              Delete
            </th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="song in songs">
            <td>
              {{song.title}}
            </td>
            <td>
              {{song.artist}}
            </td>
            <td>
              <span class="glyphicon glyphicon-trash" v-on:click="removeSong(song)"></span>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>

  </div>
</template>

<script>

import Firebase from 'firebase'


let config = {
  apiKey: "AIzaSyClpGYejDO6gy9K3eDroKGf7IbLOErF1jM",
  authDomain: "dj-song-request.firebaseapp.com",
  databaseURL: "https://dj-song-request.firebaseio.com",
  projectId: "dj-song-request",
  storageBucket: "dj-song-request.appspot.com",
  messagingSenderId: "946033756546"
};

let app = Firebase.initializeApp(config);
let db = app.database();

let songsRef = db.ref("songs");

export default {
  name: 'hello',
  data () {
    return {
      msg: 'DJ song request',
      newSong: {
        title: '',
        artist: ''
      }
    }
  },
  methods: {
    addSong: function(){
      songsRef.push(this.newSong);
      this.newSong.title = '';
      this.newSong.artist = '';
    },
    removeSong: function(song){
      songsRef.child(song['.key']).remove();
    }
  },
  firebase: {
    songs: songsRef
  }
}

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
</style>
