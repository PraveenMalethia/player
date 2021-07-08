<template>
  <v-container class="pa-4 text-center">
    <v-row class="fill-height" align="center" justify="center">
      <template>
        <v-col cols="12" md="4">
          <v-hover v-slot="{ hover }">
            <v-card
              :elevation="hover ? 12 : 2"
              class="on-hover"
              color="green lighten-2"
            >
              <v-card-title class="text-h6">
                <v-row class="fill-height flex-column" justify="space-between">
                  <p class="mt-4 ml-4 subheading text-center">
                    {{ song.name }}
                    <v-icon v-if="playing" class="ml-2">mdi-music</v-icon>
                  </p>
                  <v-slider
                    v-model="volume"
                    :prepend-icon="muted ? 'mdi-volume-off' : 'mdi-volume-high'"
                    @click:prepend="mute(song.id)"
                    color="green darken-4"
                    @change="setVolume(song.id, volume)"
                    track-color="white"
                    class="ml-4 mr-4"
                    thumb-color="green darken-4"
                    min="0"
                    max="9"
                  ></v-slider>
                  <div class="align-self-center">
                    <v-btn
                      @click="backward(song.id)"
                      :key="1"
                      class="show-btns"
                      :color="transparent"
                      icon
                    >
                      <v-icon
                        :class="{ 'show-btns': hover }"
                        :color="transparent"
                      >
                        mdi-rewind
                      </v-icon>
                    </v-btn>
                    <v-btn
                      @click="togglePlay(song.id)"
                      :key="2"
                      :class="{ 'show-btns': hover }"
                      :color="transparent"
                      icon
                    >
                      <v-icon
                        :class="{ 'show-btns': hover }"
                        :color="transparent"
                      >
                        {{ playing ? "mdi-pause" : "mdi-play" }}
                      </v-icon>
                    </v-btn>
                    <v-btn
                      @click="forward(song.id)"
                      :key="3"
                      :class="{ 'show-btns': hover }"
                      :color="transparent"
                      icon
                    >
                      <v-icon
                        :class="{ 'show-btns': hover }"
                        :color="transparent"
                      >
                        mdi-fast-forward
                      </v-icon>
                    </v-btn>
                    <v-btn
                      @click="repeat(song.id)"
                      :key="4"
                      :class="{ 'show-btns': hover }"
                      :color="transparent"
                      icon
                    >
                      <v-icon
                        :class="{ 'show-btns': hover }"
                        :color="transparent"
                      >
                        {{ loop ? 'mdi-repeat' : 'mdi-repeat-off' }}
                      </v-icon>
                    </v-btn>
                  </div>
                </v-row>
              </v-card-title>
            </v-card>
          </v-hover>
        </v-col>
        <audio :id="'music' + song.id">
          <source :src="`http://localhost:8000${song.song}`" type="audio/ogg" />
        </audio>
      </template>
    </v-row>
  </v-container>
</template>
<script>
export default {
  props: {
    song: {
      type: Object,
      required: true,
    },
  },
  data: () => ({
    playing: false,
    transparent: "black",
    otherPlaying: false,
    volume: 9,
    muted: false,
    loop:false,
  }),
  mounted() {
    console.log(document.getElementById("music" + this.song.id).currentTime);
  },
  methods: {
    backward(id) {
      let music = document.getElementById("music" + id);
      music.currentTime = music.currentTime - 10;
    },

    togglePlay(id) {
      let music = document.getElementById("music" + id);
      music.play();
      this.otherPlaying = true;
      if (this.playing) {
        music.pause();
        this.playing = false;
      } else {
        music.play();
        this.playing = true;
      }
    },

    forward(id) {
      var music = document.getElementById("music" + id);
      var time = music.currentTime + 10.0;
      console.log(time);
      music.currentTime = time;
    },
    mute(id) {
      let music = document.getElementById("music" + id);
      if (this.muted) {
        music.muted = false;
        this.muted = false;
      } else {
        music.muted = true;
        this.muted = true;
      }
    },
    repeat(id){
      let music = document.getElementById("music" + id);
      if (music.loop){
        music.loop = false;
        this.loop = false;
      }
      else{
        music.loop = true;
        this.loop = true;
      }
      console.log(music.loop);
    },
    intToFloat(num, decPlaces) {
      return 0 + "." + String(num)[0] + Array(decPlaces + 2).join("0");
    },
    setVolume(id, volume) {
      let vol = this.intToFloat(volume, 1);
      if (Number(vol) == 0) {
        this.muted = true;
      } else {
        this.muted = false;
      }
      var music = document.getElementById("music" + id);
      music.volume = Number(vol);
    },
  },
};
</script>
<style scoped>
.v-card {
  transition: opacity 0.4s ease-in-out;
}

.v-card:not(.on-hover) {
  opacity: 0.6;
}

.show-btns {
  color: rgba(255, 255, 255, 1) !important;
}
</style>