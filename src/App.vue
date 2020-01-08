<template>
  <div id="app">
    <header>
      <h1>Vue Music</h1>
    </header>
    <main>
      <section class="player">
        <h2 class="song-title">{{ current.title }}</h2>
        <p class="song-artist">{{ current.artist }}</p>
        <div class="controls">
          <button class="prev" @click="prev" title="Previous"><i class="fa fa-angle-double-left"></i></button>
          <button class="play" v-if="!isPlaying" @click="play" title="Play"><i class="fa fa-play fa-lg"></i></button>
          <button class="pause" v-else @click="pause" title="Pause"><i class="fa fa-pause fa-lg"></i></button>
          <button class="next" @click="next" title="Next"><i class="fa fa-angle-double-right"></i></button>
        </div>
      </section>
      <section class="playlist">
        <!-- <h3>The Playlist</h3> -->
        <button v-for="song in songs" :key="song.src" @click="play(song)" :class="(song.src == current.src) ? 'song playing' : 'song'">
          {{ song.title }}
        </button>
      </section>
    </main>
  </div>
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
      current: {},
      index: 0,
      isPlaying: false,
      songs: [
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
          title: 'Better Days',
          artist: 'Benjamin Tissot',
          src: require('./assets/bensound-betterdays.mp3')
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
    play (song) {
      if (typeof song.src != "undefined") {
        this.current = song;

        this.player.src = this.current.src;
      }

      this.player.play();
      this.player.addEventListener('ended', function () {
        this.index++;
        if (this.index > this.songs.length - 1) {
          this.index = 0;
        }

        this.current = this.songs[this.index];
        this.play(this.current);
      }.bind(this));
      this.isPlaying = true;
    },
    pause () {
      this.player.pause();
      this.isPlaying = false;
    },
    next () {
      this.index++;
      if (this.index > this.songs.length - 1) {
        this.index = 0;
      }

      this.current = this.songs[this.index];
      this.play(this.current);
    },
    prev () {
      this.index--;
      if (this.index < 0) {
        this.index = this.songs.length - 1;
      }

      this.current = this.songs[this.index];
      this.play(this.current);
    }
  },
  created () {
    this.current = this.songs[this.index];
    this.player.src = this.current.src;
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
	color: #FFF;
}
main {
  width: 100%;
  max-width: 768px;
  margin: 0 auto;
  padding: 25px;
}

.song-title {
  color: #53565A;
  font-size: 28px;
  font-weight: 700;
  text-transform: uppercase;
  text-align: center;
}
.song-artist {
  color: #53565A;
  font-size: 14px;
  font-weight: 400;
  font-style: italic;
  text-align: center;
}

.controls {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 30px 15px;
}

button {
  appearance: none;
  background: none;
  border: none;
  outline: none;
  cursor: pointer;
}
button:hover {
  opacity: 0.8;
}

.play, .pause {
  font-size: 20px;
  font-weight: 700;
  padding: 15px 25px;
  margin: 0px 15px;
  border-radius: 8px;
  color: #FFF;
  background-color: #CC2E5D;
}

.next, .prev {
  font-size: 16px;
  font-weight: 700;
  padding: 10px 20px;
  margin: 0px 15px;
  border-radius: 6px;
  color: #FFF;
  background-color: #FF5858;
}

.playlist {
  padding: 0px 30px;
}
.playlist h3 {
  color: #212121;
  font-size: 28px;
  font-weight: 400;
  margin-bottom: 30px;
  text-align: center;
}
.playlist .song {
  display: block;
  width: 100%;
  padding: 15px;
  font-size: 20px;
  font-weight: 700;
  cursor: pointer;
}
.playlist .song:hover {
  color: #FF5858;
}

.playlist .song.playing {
  color: #FFF;
  background-image: linear-gradient(to right, #CC2E5D, #FF5858);
}
</style>