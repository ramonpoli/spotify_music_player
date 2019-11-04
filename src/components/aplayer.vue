<template>
  <div class="aplayer container">
    <div class="aplayer-header">
      <div class="aplayer-options-menu">
        <div class="aplayer-options-trigger">
          <span>. . .</span>
        </div>
      </div>
      <span class="title">Spotify Web</span>
    </div>
    <div class="aplayer-song-list">
      <div
        class="aplayer-song"
        v-for="song in songs"
        :key="song.id"
        @click="playSong(song)"
        v-bind:class="{playing: song === songPlaying}"
      >
        <div class="aplayer-song__play-button" v-bind:class="{pause: song === songPlaying}"></div>
        <div class="aplayer-song__song-name">{{song.songTitle}}</div>
        <div class="aplayer-song__song-artist">{{song.artist}}</div>
      </div>
    </div>
    <div class="aplayer-display" :class="{'hover': isHovering}">
      <div class="song-title">
        <span>{{songPlaying.songTitle}}</span>
      </div>
      <div class="song-artist">
        <span>{{songPlaying.artist}}</span>
      </div>
      <div class="scroller-container">
        <div class="scroller-background"></div>
        <div class="scroller" v-bind:style="{ width: percentageOfSong + '%' }"></div>
        <div class="scroller-indicator" v-bind:style="{ marginLeft: percentageOfSong - 2 + '%' }">
          
        </div>
      </div>
      <div class="aplayer-timer">
        <div class="aplayer-timer__start">
          <span>{{timerRunning}}</span>
        </div>
        <div class="aplayer-timer__stop">
          <span>{{songPlaying.duration}}</span>
        </div>
      </div>
    </div>
    <audio ref="audio"></audio>
  </div>
</template>

<script>
import Songs from '../assets/songs.json';

export default {
  name: 'aplayer',
  components: {},
  data() {
    return {
      songs: Songs,
      isHovering: false,
      percentageOfSong: 0,
      timerRunning: '0:00',
      songPlaying: {},
    };
  },
  methods: {
    playSong(song) {
      this.songPlaying = song;
      this.everySecondTimer = setInterval(this.everySecond, 1000);
    },
    everySecond() {
      const timeSplitted = this.timerRunning.split(':');
      let minutes = parseInt(timeSplitted[0]);
      let seconds = parseInt(timeSplitted[1]);
      if (seconds === 59) {
        minutes = minutes === 59 ? 0 : minutes + 1;
        seconds = 0;
      } else {
        seconds++;
      }
      this.timerRunning = `${minutes}:${seconds < 10 ? '0' : ''}${seconds}`;
      this.percentageOfSong = ((10 / this.songPlaying.duration.split(':')[0]) * minutes
          + seconds / 100)
        * 10;
    },
  },
};
</script>

<style lang="scss">
* {
  color: #fff;
}
.aplayer.container {
  background: #191414;
  width: 350px;
  margin: 70px auto;
  border-radius: 4px;
  color: #fff;
  box-shadow: 0 15px 45px rgba(25, 20, 20, 0.4);
  padding: 0px;
}
.aplayer-header {
  text-align: center;
  padding: 25px;
  color: white;
  border: 1px solid rgba(255, 255, 255, 0.2);
  .aplayer-options-menu {
    position: absolute;
    cursor: pointer;
    .aplayer-options-trigger {
      border-radius: 50%;
      height: 25px;
      width: 25px;
      border: 1px solid #fff;
      margin: -5px 0 0 0;
      span {
        top: -4px;
        position: relative;
        display: inline-block;
        line-height: 20px;
        font-size: 0.7em;
      }
    }
  }
}
.aplayer-song-list {
  height: 350px;
  width: 350px;
  background-size: cover;
  .aplayer-song {
    border-bottom: 1px solid rgba(255, 255, 255, 0.2);
    height: 40px;
    padding: 10px 15px;
    display: flex;
    flex-direction: row;
    align-content: space-around;
    font-size: 0.8rem;
    &__play-button {
      margin-top: 4px;
      flex-grow: 0;
      box-sizing: border-box;
      width: 12px;
      height: 12px;
      border-style: solid;
      border-width: 6px 0px 6px 12px;
      border-color: transparent transparent transparent white;
      &.pause {
        border-style: double;
        border-width: 0px 0 0px 10px;
      }
    }
    &__song-name {
      flex-grow: 1;
      text-align: left;
      padding-left: 12px;
      line-break: normal;
    }
    &__song-artist {
      flex-grow: 1;
      text-align: right;
    }
    &.playing {
      background: #282828;
    }
  }
}
.aplayer-display {
  background: #282828;
  padding: 20px 30px;
  border-radius: 4px;
  font-size: 0.8rem;
  &.hover {
    background: #ffffff;
    opacity: 1;
  }
  .song-title {
    font-size: 1.2em;
  }
  .song-artist {
    font-size: 1em;
    padding: 10px;
    color: rgba(255, 255, 255, 0.5);
    cursor: pointer;
    & span:hover {
      color: rgba(255, 255, 255, 1);
    }
  }
  .scroller-container {
    .scroller-background {
      background: #404040;
      height: 8px;
      border-radius: 25px;
      z-index: 0;
    }
    .scroller {
      background: #1ed660;
      width: 60%;
      height: 8px;
      border-radius: 25px;
      margin: -8px 0 0 0;
      z-index: 1;
    }
    .scroller-indicator {
      background: #1ed660;
      width: 14px;
      height: 14px;
      border-radius: 25px;
      margin: -11px 0 0 57%;
      z-index: 2;
      box-shadow: 0 0 5px 2px rgba(165, 35, 35, 0.3);
      cursor: pointer;
      opacity: 0;
      transition: background 100ms ease, box-shadow 100ms ease,
        opacity 100ms ease;
      &:hover {
        background: #fff;
        opacity: 1;
      }
    }
  }
}
.aplayer-timer {
  display: flex;
  justify-content: space-between;
}
</style>
