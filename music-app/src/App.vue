<template>
  <div>
    <header>
      <h1>My music</h1>
    </header>
    <main>
      <section class="player">
        <h2 class="song-title">
          {{ current.title }} - <span>{{ current.artist }}</span>
        </h2>
        <div class="controls">
          <button class="prev" @click="prev">prev</button>
          <button class="play" @click="play" v-if="!isPlaying">play</button>
          <button class="pause" @click="pause" v-else>pause</button>
          <button class="next" @click="next">next</button>
        </div>
      </section>

      <section class="playlist">
        <h3>The Playlist</h3>
        <button
          v-for="song in songs"
          :key="song.src"
          @click="play(song)"
          :class="song.src == current.src ? 'song playing' : 'song'"
        >
          {{ song.title }} - {{ song.artist }}
        </button>
      </section>
    </main>
  </div>
</template>

<script>
// import HomePage from "./components/HomePage";
export default {
  name: "App",
  data() {
    return {
      current: "",
      index: 0,
      isPlaying: false,
      songs: [
        {
          title: "Ei Obelay",
          artist: "Shironamhin",
          src: require("./assets/Ei-obelay.mp3"),
        },
        {
          title: "Deewane Hum Nehi Hote",
          artist: "Aditya Srivastav",
          src: require("./assets/Deewane.mp3"),
        },
      ],
      player: new Audio(),
    };
  },
  methods: {
    play(song) {
      if (typeof song.src != "undefined") {
        this.current = song;
        this.player.src = this.current.src;
      }
      this.player.play();
      this.player.addEventListener(
        "ended",
        function () {
          this.index++;
          if (this.index > this.songs.length - 1) {
            this.index = 0;
          }
          this.current = this.songs[this.index];
          this.play(this.current);
        }.bind(this)
      );
      this.isPlaying = true;
    },
    pause() {
      this.player.pause();
      this.isPlaying = false;
    },
    next() {
      this.index++;
      if (this.index > this.songs.length - 1) {
        this.index = 0;
      }
      this.current = this.songs[this.index];
      this.play(this.current);
    },
    prev() {
      this.index--;
      if (this.index < 0) {
        this.index = this.songs.length - 1;
      }
      this.current = this.songs[this.index];
      this.play(this.current);
    },
  },
  created() {
    this.current = this.songs[this.index];
    this.player.src = this.current.src;
  },
};
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
  width: 100%;
  background: rgb(34, 34, 34);
  color: #fff;
  padding: 15px;
}
main {
  margin: 0 auto;
  width: 100%;
  max-width: 768px;
  text-align: center;
  padding: 15px;
  background: #f3f7fd;
  box-shadow: 0px 0px 11px 5px #b0b0b0;
  margin-top: 20px;
}
.song-title {
  font-size: 28px;
  color: #464646;
  text-transform: uppercase;
}
.song-title span {
  font-weight: 400;
  font-style: italic;
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
.play,
.pause {
  font-size: 20px;
  padding: 15px 25px;
  border-radius: 5px;
  margin: 0 15px;
  background-image: linear-gradient(8deg, #701186, #0dcfcf);
  color: #fff;
}
.play:hover {
  opacity: 0.8;
}
.prev,
.next {
  background-image: linear-gradient(38deg, #2fca42, #1c237af0);
  padding: 10px 20px;
  color: #fff;
  border-radius: 5px;
  font-size: 17px;
}
.playlist {
  padding: 0px 30px;
}

.playlist h3 {
  font-size: 28px;
  color: #414141;
  font-weight: 400;
  margin-bottom: 30px;
}
.song {
  font-size: 17px;
  width: 100%;
  display: block;
  cursor: pointer;
  padding: 15px;
}
.song.playing {
  background: #436cb2;
  border-radius: 5px;
  color: #fff;
  transition: 0.3s background;
  transition-delay: 0.1s;
}
</style>
