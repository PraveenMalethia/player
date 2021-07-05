<template>
  <v-app>
    <v-app-bar
      app
      color="deep-purple darken-1"
      dark
    >
      <div class="d-flex align-center">
        <h2>Esportify</h2>
      </div>

      <v-spacer></v-spacer>
    </v-app-bar>

    <v-main >
      <v-container class="pa-4 text-center">
        <h2>Your Playlist</h2>
        <v-btn
      class="ma-2"
      :loading="loading"
      :disabled="loading"
      color="secondary"
      @click="LoadSongs"
    >
      Refresh Playlist
    </v-btn>
      </v-container>
      <div v-for="item in items" :key="item.id">
        <Song :song="item"/>
      </div>
    </v-main>
  </v-app>
</template>

<script>
import Song from './components/Song';
import axios from 'axios';

export default {
  name: 'App',

  components: {
    Song,
  },

  data: () => ({
    items:[],
    loading:false,
  }),
  mounted () {
    this.LoadSongs()
    },
  methods:{
    LoadSongs(){
      this.loading = true
      axios
      .get('http://localhost:8000/')
        .then(response => {
          this.items = response.data
          this.loading = false
    })
    }
  }
};
</script>
