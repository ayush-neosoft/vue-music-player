<template>
  <div id="app">
    <header>
      <h1>My Music</h1>
    </header>
    <main>
      <section class="player">
        <h2 class="song-title">{{ current.title }} - <span>{{ current.artist }}</span></h2>
        <div class="controls">
          <button class="prev" @click="prev">Prev</button>
          <button class="play" v-if="!isPlaying" @click="play">Play</button>
          <button class="pause" v-else @click="pause">Pause</button>
          <button class="next" @click="next">Next</button>
        </div>
      </section>
      <section class="playlist">
        <h3>The Playlist</h3>
        <button v-for="(song, index) in songs" :key="index" @click="play(song)" :class="(song.src == current.src) ? 'song-playing' : 'song'">
          {{ song.title }} - {{ song.artist }}
        </button>
      </section>
    </main>
  </div>
</template>

<script>

export default {
  name: 'app',
  created() {
    this.current = this.songs[this.index];
    this.player.src = this.current.src;
  },
  data() {
    return {
      current: {},
      index: 0,
      isPlaying: false,
      songs: [
        {
          title: 'Better Days',
          artist: 'Benjamin Tissot',
          src: require('./assets/bensound-betterdays.mp3')
        },
        {
          title: 'Buddy',
          artist: 'Benjamin Tissot',
          src: require('./assets/bensound-buddy.mp3')
        },
        {
          title: 'Dubstep',
          artist: 'Benjamin Tissot',
          src: require('./assets/bensound-dubstep.mp3')
        },
        {
          title: 'Energy',
          artist: 'Benjamin Tissot',
          src: require('./assets/bensound-energy.mp3')
        },
        {
          title: 'Sunny',
          artist: 'Benjamin Tissot',
          src: require('./assets/bensound-sunny.mp3')
        }
      ],
      player: new Audio()
    }
  },
  methods: {
    play(song) {
      if(typeof song.src != "undefined") {
        this.current = song;

        this.player.src = this.current.src
      }

      this.player.play();
      this.isPlaying = true;
    },
    pause() {
      this.player.pause();
      this.isPlaying = false;
    },
    next() {
      this.index++;
      if(this.index > this.songs.length - 1){
        this.index = 0
      }

      this.current = this.songs[this.index];
      this.play(this.current);  
    },
    prev() {
      this.index--;
      if(this.index < 0){
        this.index = this.songs.length - 1
      }

      this.current = this.songs[this.index];
      this.play(this.current);  
    }
  }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: sans-serif;
}

header {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 15px;
  background-color: #212121;
  color: #FFF
}
</style>
