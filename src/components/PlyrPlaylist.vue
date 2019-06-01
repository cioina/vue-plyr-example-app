<template>
  <div class="container" v-if="playlistLoaded">
    <vue-plyr ref="player" :options="options">
      <div
        class="js-player"
        :data-plyr-provider="nowPlaying.source.type"
        :data-plyr-embed-id="nowPlaying.source.src"
      ></div>
    </vue-plyr>

    <div class="plyr-playlist-wrapper">
      <ul class="plyr-playlist">
        <li
          @click="playVideo(index)"
          :class="
            nowPlaying.source.src === video.source.src ? 'pls-playing' : ''
          "
          v-for="(video, index) in playlist"
          :key="video.source.src"
        >
          <a
            href="#"
            :data-plyr-provider="video.source.type"
            :data-plyr-embed-id="video.source.src"
          >
            <img class="plyr-miniposter" :src="video.poster" />
            {{ video.title }}
          </a>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
import $ from "jquery";
import VueScrollTo from "vue-scrollto";

export default {
  data() {
    return {
      options: {
        controls: `
        <div class="plyr__controls">
            <button type="button" class="plyr__control" data-plyr="prev">
                <i class="fa fa-step-backward fa-lg"></i>
                <span class="plyr__tooltip" role="tooltip">Previous Video</span>
            </button>
            <button type="button" class="plyr__control" aria-label="Play, {title}" data-plyr="play">
                <svg class="icon--pressed" role="presentation"><use xlink:href="#plyr-pause"></use></svg>
                <svg class="icon--not-pressed" role="presentation"><use xlink:href="#plyr-play"></use></svg>
                <span class="label--pressed plyr__tooltip" role="tooltip">Pause</span>
                <span class="label--not-pressed plyr__tooltip" role="tooltip">Play</span>
            </button>
            <button type="button" class="plyr__control" data-plyr="next">
                <i class="fa fa-step-forward fa-lg"></i>
                <span class="plyr__tooltip" role="tooltip">Next Video</span>
            </button>
            <div class="plyr__progress">
                <input data-plyr="seek" type="range" min="0" max="100" step="0.01" value="0" aria-label="Seek">
                <progress class="plyr__progress__buffer" min="0" max="100" value="0">% buffered</progress>
                <span role="tooltip" class="plyr__tooltip">00:00</span>
            </div>
            <div class="plyr__time plyr__time--current" aria-label="Current time">00:00</div>
            <div class="plyr__time plyr__time--duration" aria-label="Duration">00:00</div>
            <button type="button" class="plyr__control" aria-label="Mute" data-plyr="mute">
                <svg class="icon--pressed" role="presentation"><use xlink:href="#plyr-muted"></use></svg>
                <svg class="icon--not-pressed" role="presentation"><use xlink:href="#plyr-volume"></use></svg>
                <span class="label--pressed plyr__tooltip" role="tooltip">Unmute</span>
                <span class="label--not-pressed plyr__tooltip" role="tooltip">Mute</span>
            </button>
            <div class="plyr__volume">
                <input data-plyr="volume" type="range" min="0" max="1" step="0.05" value="1" autocomplete="off" aria-label="Volume">
            </div>
            <button type="button" class="plyr__control" data-plyr="captions">
                <svg class="icon--pressed" role="presentation"><use xlink:href="#plyr-captions-on"></use></svg>
                <svg class="icon--not-pressed" role="presentation"><use xlink:href="#plyr-captions-off"></use></svg>
                <span class="label--pressed plyr__tooltip" role="tooltip">Disable captions</span>
                <span class="label--not-pressed plyr__tooltip" role="tooltip">Enable captions</span>
            </button>
            <button type="button" class="plyr__control" data-plyr="fullscreen">
                <svg class="icon--pressed" role="presentation"><use xlink:href="#plyr-exit-fullscreen"></use></svg>
                <svg class="icon--not-pressed" role="presentation"><use xlink:href="#plyr-enter-fullscreen"></use></svg>
                <span class="label--pressed plyr__tooltip" role="tooltip">Exit fullscreen</span>
                <span class="label--not-pressed plyr__tooltip" role="tooltip">Enter fullscreen</span>
            </button>
        </div>        
        `
      },

      /* PLAYLIST  */
      playlistLoaded: false,
      nowPlaying: null,
      playlist: [
        {
          title: "Maroon 5 - Girls Like You ft. Cardi B",
          source: { src: "aJOTlE1K90k", type: "youtube" },
          poster: "https://i.ytimg.com/vi/aJOTlE1K90k/mqdefault.jpg"
        },
        {
          title: "Ed Sheeran - Perfect (Official Music Video)",
          source: { src: "2Vv-BfVoq4g", type: "youtube" },
          poster: "https://i.ytimg.com/vi/2Vv-BfVoq4g/mqdefault.jpg"
        },
        {
          title:
            "Bebe Rexha - Meant to Be (feat. Florida Georgia Line) [Official Music Video]",
          source: { src: "zDo0H8Fm7d0", type: "youtube" },
          poster: "https://i.ytimg.com/vi/zDo0H8Fm7d0/mqdefault.jpg"
        },
        {
          title: "Bazzi feat. Camila Cabello - Beautiful (Official Video)",
          source: { src: "Uk1hv6h7O1Y", type: "youtube" },
          poster: "https://i.ytimg.com/vi/Uk1hv6h7O1Y/mqdefault.jpg"
        },
        {
          title:
            "Silk City, Dua Lipa - Electricity (Official Video) ft. Diplo, Mark Ronson",
          source: { src: "Q4-jOuHO-z4", type: "youtube" },
          poster: "https://i.ytimg.com/vi/Q4-jOuHO-z4/mqdefault.jpg"
        },
        {
          title: "Dua Lipa - New Rules (Official Music Video)",
          source: { src: "k2qgadSvNyU", type: "youtube" },
          poster: "https://i.ytimg.com/vi/k2qgadSvNyU/mqdefault.jpg"
        },
        {
          title: "Calum Scott - You Are The Reason (Official)",
          source: { src: "ShZ978fBl6Y", type: "youtube" },
          poster: "https://i.ytimg.com/vi/ShZ978fBl6Y/mqdefault.jpg"
        },
        {
          title: 'Charlie Puth - "How Long" [Official Video]',
          source: { src: "CwfoyVa980U", type: "youtube" },
          poster: "https://i.ytimg.com/vi/CwfoyVa980U/mqdefault.jpg"
        },
        {
          title: "Selena Gomez, Marshmello - Wolves (Official Music Video)",
          source: { src: "cH4E_t3m3xM", type: "youtube" },
          poster: "https://i.ytimg.com/vi/cH4E_t3m3xM/mqdefault.jpg"
        },
        {
          title: "Imagine Dragons - Whatever It Takes (Official Music Video)",
          source: { src: "gOsM-DYAEhY", type: "youtube" },
          poster: "https://i.ytimg.com/vi/gOsM-DYAEhY/mqdefault.jpg"
        },
        {
          title: "Imagine Dragons - Thunder",
          source: { src: "fKopy74weus", type: "youtube" },
          poster: "https://i.ytimg.com/vi/fKopy74weus/mqdefault.jpg"
        },
        {
          title: "ZAYN - Dusk Till Dawn (Official Video) ft. Sia",
          source: { src: "tt2k8PGm-TI", type: "youtube" },
          poster: "https://i.ytimg.com/vi/tt2k8PGm-TI/mqdefault.jpg"
        },
        {
          title: "Clean Bandit - Symphony feat. Zara Larsson [Official Video]",
          source: { src: "aatr_2MstrI", type: "youtube" },
          poster: "https://i.ytimg.com/vi/aatr_2MstrI/mqdefault.jpg"
        },
        {
          title: "Luis Fonsi, Demi Lovato - Échame La Culpa (Video Oficial)",
          source: { src: "TyHvyGVs42U", type: "youtube" },
          poster: "https://i.ytimg.com/vi/TyHvyGVs42U/mqdefault.jpg"
        },
        {
          title: "Juice WRLD - Lucid Dreams (Dir. by @_ColeBennett_)",
          source: { src: "mzB1VGEGcSU", type: "youtube" },
          poster: "https://i.ytimg.com/vi/mzB1VGEGcSU/mqdefault.jpg"
        },
        {
          title: "Taylor Swift - Look What You Made Me Do",
          source: { src: "3tmd-ClpJxA", type: "youtube" },
          poster: "https://i.ytimg.com/vi/3tmd-ClpJxA/mqdefault.jpg"
        },
        {
          title: "Rita Ora - Anywhere (Official Video)",
          source: { src: "ksdAs4LBRq8", type: "youtube" },
          poster: "https://i.ytimg.com/vi/ksdAs4LBRq8/mqdefault.jpg"
        },
        {
          title: "Shawn Mendes - There's Nothing Holdin' Me Back",
          source: { src: "dT2owtxkU8k", type: "youtube" },
          poster: "https://i.ytimg.com/vi/dT2owtxkU8k/mqdefault.jpg"
        },
        {
          title: "Camila Cabello - Havana (Vertical Video) ft. Young Thug",
          source: { src: "pz95u3UVpaM", type: "youtube" },
          poster: "https://i.ytimg.com/vi/pz95u3UVpaM/mqdefault.jpg"
        },
        {
          title: "Bryce Vine - Drew Barrymore [Official Music Video]",
          source: { src: "kudi8OtMu9s", type: "youtube" },
          poster: "https://i.ytimg.com/vi/kudi8OtMu9s/mqdefault.jpg"
        },
        {
          title: "Dynoro, Gigi D'Agostino - In My Mind (Official Audio)",
          source: { src: "q0pdLCQy8l8", type: "youtube" },
          poster: "https://i.ytimg.com/vi/q0pdLCQy8l8/mqdefault.jpg"
        },
        {
          title:
            "Luis Fonsi - Despacito ft. Daddy Yankee (Official Music Video)",
          source: { src: "kJQP7kiw5Fk", type: "youtube" },
          poster: "https://i.ytimg.com/vi/kJQP7kiw5Fk/mqdefault.jpg"
        },
        {
          title:
            "Katy Perry - Chained To The Rhythm (Official) ft. Skip Marley",
          source: { src: "Um7pMggPnug", type: "youtube" },
          poster: "https://i.ytimg.com/vi/Um7pMggPnug/mqdefault.jpg"
        },
        {
          title: "Kygo, Selena Gomez - It Ain't Me",
          source: { src: "u3VTKvdAuIY", type: "youtube" },
          poster: "https://i.ytimg.com/vi/u3VTKvdAuIY/mqdefault.jpg"
        },
        {
          title: "Charlie Puth - Attention [Official Video]",
          source: { src: "nfs8NYg7yQM", type: "youtube" },
          poster: "https://i.ytimg.com/vi/nfs8NYg7yQM/mqdefault.jpg"
        },
        {
          title: "Kygo, Miguel - Remind Me to Forget",
          source: { src: "FRjOSmc01-M", type: "youtube" },
          poster: "https://i.ytimg.com/vi/FRjOSmc01-M/mqdefault.jpg"
        }
      ]
    };
  },

  computed: {
    player() {
      return this.$refs.player.player;
    }
  },

  methods: {
    initPlaylist() {
      this.playlistLoaded = true;
    },

    setupPlaylistListeners() {
      $("body").off("click", '.plyr__controls [data-plyr="prev"]');
      $("body").off("click", '.plyr__controls [data-plyr="next"]');

      $("body")
        .on("click", '.plyr__controls [data-plyr="prev"]', () => {
          if (this.playlistIndex > 0) {
            this.playlistIndex -= 1;
            this.playVideo(this.playlistIndex);
          }
        })
        .on("click", '.plyr__controls [data-plyr="next"]', () => {
          if (this.playlistIndex < this.playlist.length - 1) {
            this.playlistIndex += 1;
            this.playVideo(this.playlistIndex);
          }
        });
    },

    playVideo(index) {
      this.playlistIndex = index;
      this.nowPlaying = this.playlist[index];

      const newSource = {
        type: "video",
        sources: [
          {
            src: this.nowPlaying.source.src,
            provider: this.nowPlaying.source.type
          }
        ]
      };

      this.player.source = newSource;

      setTimeout(() => {
        VueScrollTo.scrollTo(".pls-playing", {
          duration: 300,
          container: ".plyr-playlist"
        });
      }, 500);
    }
  },

  watch: {
    playlistLoaded() {
      if (this.playlistLoaded) {
        // load first video from playlist
        this.playlistIndex = 0;
        this.nowPlaying = this.playlist[this.playlistIndex];

        this.$nextTick(() => {
          this.player.on("ready", () => {
            this.setupPlaylistListeners();
          });
        });
      }
    }
  },

  mounted() {
    this.initPlaylist();
  }
};
</script>

<style>
.container {
  max-width: 600px;
  margin: 2rem auto;
}

/* Playlist  */
/* scrollbar rules have to be separate, browsers not supporting this syntax will skip them when combined. */
.plyr-playlist-wrapper ul::-webkit-scrollbar {
  width: 6px;
}

.plyr-playlist-wrapper ul::-webkit-scrollbar-track {
  background: rgba(0, 0, 0, 0.3);
  border-radius: 10px;
  -moz-border-radius: 10px;
  -webkit-border-radius: 10px;
}

.plyr-playlist-wrapper ul::-webkit-scrollbar-thumb {
  border-radius: 10px;
  -moz-border-radius: 10px;
  -webkit-border-radius: 10px;
  /*  background: #fff; */
  background: #3498db;
}
.plyr-playlist-wrapper {
  background: rgba(0, 0, 0, 0.8);
  position: relative;
  padding: 0.5em 0.5em 0.5em 0.25em;
}
.plyr-playlist-wrapper .plyr-playlist {
  user-select: none;
  -webkit-tap-highlight-color: rgba(0, 0, 0, 0);
  position: relative;
  margin-top: 0;
  padding: 6px 4px;
  width: 100%;
  box-sizing: border-box;
  -moz-box-shadow: rgba(0, 0, 0, 0.2) 0px 6px 8px 0px inset;
  -webkit-box-shadow: rgba(0, 0, 0, 0.2) 0px 6px 8px 0px inset;
  box-shadow: rgba(0, 0, 0, 0.2) 0px 6px 8px 0px inset;
}

.plyr-playlist-wrapper ul {
  padding: 0;
  margin: 0;
  max-height: 12em;
  overflow-y: scroll;
  -webkit-overflow-scrolling: touch;
}

.plyr-playlist-wrapper ul li {
  border-bottom: 1px solid rgba(0, 0, 0, 0.05);
}

.plyr-playlist-wrapper .plyr-playlist li {
  list-style: none;
  background-color: rgba(255, 255, 255, 0.03);
  padding: 0px;
  margin-bottom: 3px;
  font-size: 90%;
}

.plyr-playlist-wrapper .plyr-playlist li.pls-playing,
.plyr-playlist-wrapper .plyr-playlist li:hover {
  color: #3498db;
  background-color: rgba(255, 255, 255, 0.09);
}
.plyr-playlist-wrapper .plyr-playlist li.pls-playing a {
  color: #3498db;
}

.plyr-playlist-wrapper .plyr-playlist li a {
  text-decoration: none;
  font-family: arial;
  color: #c9c9c9;
  display: block;
  padding: 10px 0;
  outline: none;
  padding: 0.5em 0.25em 0.5em 0.75em;
  font-size: 90%;
  vertical-align: middle;
  padding-bottom: 10px;
}

.plyr-playlist-wrapper .plyr-playlist li:last-child a {
  border-bottom: 0;
}

.plyr-playlist li a:hover,
.plyr-playlist li a:focus,
.plyr-playlist li a:active {
  color: #04a9f3;
}

.plyr-miniposter {
  width: auto;
  height: 22px;
  -webkit-background-size: cover;
  background-size: cover;
  background-repeat: no-repeat;
  background-position: center center;
  float: left;
  margin-right: 10px;
  border-radius: 0;
  -moz-border-radius: 0;
  -webkit-border-radius: 0;
}

/* YOUTUBE HIDE BLACK BARS https://stackoverflow.com/a/33604743/211324 */
/* https://jsfiddle.net/onigetoc/nomzb6hf/ */

@keyframes plyr-progress {
  to {
    background-position: 25px 0;
  }
}
@keyframes plyr-popup {
  0% {
    opacity: 0.5;
    transform: translateY(10px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}
@keyframes plyr-fade-in {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}
.plyr {
  -moz-osx-font-smoothing: auto;
  -webkit-font-smoothing: subpixel-antialiased;
  direction: ltr;
  font-family: Avenir, "Avenir Next", "Helvetica Neue", "Segoe UI", Helvetica,
    Arial, sans-serif;
  font-variant-numeric: tabular-nums;
  font-weight: 500;
  line-height: 1.7;
  max-width: 100%;
  min-width: 200px;
  position: relative;
  text-shadow: none;
  transition: box-shadow 0.3s ease;
}
.plyr audio,
.plyr video {
  border-radius: inherit;
  height: auto;
  vertical-align: middle;
  width: 100%;
}
.plyr button {
  font: inherit;
  line-height: inherit;
  width: auto;
}
.plyr:focus {
  outline: 0;
}
.plyr--full-ui {
  box-sizing: border-box;
}
.plyr--full-ui *,
.plyr--full-ui ::after,
.plyr--full-ui ::before {
  box-sizing: inherit;
}
.plyr--full-ui a,
.plyr--full-ui button,
.plyr--full-ui input,
.plyr--full-ui label {
  touch-action: manipulation;
}
.plyr__badge {
  background: #4f5b5f;
  border-radius: 2px;
  color: #fff;
  font-size: 9px;
  line-height: 1;
  padding: 3px 4px;
}
.plyr--full-ui ::-webkit-media-text-track-container {
  display: none;
}
.plyr__captions {
  animation: plyr-fade-in 0.3s ease;
  bottom: 0;
  color: #fff;
  display: none;
  font-size: 14px;
  left: 0;
  padding: 10px;
  position: absolute;
  text-align: center;
  transition: transform 0.4s ease-in-out;
  width: 100%;
}
.plyr__captions .plyr__caption {
  background: rgba(0, 0, 0, 0.8);
  border-radius: 2px;
  -webkit-box-decoration-break: clone;
  box-decoration-break: clone;
  line-height: 185%;
  padding: 0.2em 0.5em;
  white-space: pre-wrap;
}
.plyr__captions .plyr__caption div {
  display: inline;
}
.plyr__captions span:empty {
  display: none;
}
@media (min-width: 480px) {
  .plyr__captions {
    font-size: 16px;
    padding: 20px;
  }
}
@media (min-width: 768px) {
  .plyr__captions {
    font-size: 18px;
  }
}
.plyr--captions-active .plyr__captions {
  display: block;
}
.plyr:not(.plyr--hide-controls) .plyr__controls:not(:empty) ~ .plyr__captions {
  transform: translateY(-40px);
}
.plyr__control {
  background: 0 0;
  border: 0;
  border-radius: 3px;
  color: inherit;
  cursor: pointer;
  flex-shrink: 0;
  overflow: visible;
  padding: 7px;
  position: relative;
  transition: all 0.3s ease;
}
.plyr__control svg {
  display: block;
  fill: currentColor;
  height: 18px;
  pointer-events: none;
  width: 18px;
}
.plyr__control:focus {
  outline: 0;
}
.plyr__control.plyr__tab-focus {
  box-shadow: 0 0 0 5px rgba(26, 175, 255, 0.5);
  outline: 0;
}
a.plyr__control {
  text-decoration: none;
}
a.plyr__control::after,
a.plyr__control::before {
  display: none;
}
.plyr__control.plyr__control--pressed .icon--not-pressed,
.plyr__control.plyr__control--pressed .label--not-pressed,
.plyr__control:not(.plyr__control--pressed) .icon--pressed,
.plyr__control:not(.plyr__control--pressed) .label--pressed {
  display: none;
}
.plyr--audio .plyr__control.plyr__tab-focus,
.plyr--audio .plyr__control:hover,
.plyr--audio .plyr__control[aria-expanded="true"] {
  background: #1aafff;
  color: #fff;
}
.plyr--video .plyr__control svg {
  filter: drop-shadow(0 1px 1px rgba(0, 0, 0, 0.15));
}
.plyr--video .plyr__control.plyr__tab-focus,
.plyr--video .plyr__control:hover,
.plyr--video .plyr__control[aria-expanded="true"] {
  background: #1aafff;
  color: #fff;
}
.plyr__control--overlaid {
  background: rgba(26, 175, 255, 0.8);
  border: 0;
  border-radius: 100%;
  box-shadow: 0 1px 1px rgba(0, 0, 0, 0.15);
  color: #fff;
  display: none;
  left: 50%;
  padding: 15px;
  position: absolute;
  top: 50%;
  transform: translate(-50%, -50%);
  z-index: 2;
}
.plyr__control--overlaid svg {
  left: 2px;
  position: relative;
}
.plyr__control--overlaid:focus,
.plyr__control--overlaid:hover {
  background: #1aafff;
}
.plyr--playing .plyr__control--overlaid {
  opacity: 0;
  visibility: hidden;
}
.plyr--full-ui.plyr--video .plyr__control--overlaid {
  display: block;
}
.plyr--full-ui ::-webkit-media-controls {
  display: none;
}
.plyr__controls {
  align-items: center;
  display: flex;
  justify-content: flex-end;
  text-align: center;
}
.plyr__controls .plyr__progress__container {
  flex: 1;
}
.plyr__controls .plyr__controls__item {
  margin-left: 2.5px;
}
.plyr__controls .plyr__controls__item:first-child {
  margin-left: 0;
  margin-right: auto;
}
.plyr__controls .plyr__controls__item.plyr__progress__container {
  padding-left: 2.5px;
}
.plyr__controls .plyr__controls__item.plyr__time {
  padding: 0 5px;
}
.plyr__controls .plyr__controls__item.plyr__progress__container:first-child,
.plyr__controls .plyr__controls__item.plyr__time + .plyr__time,
.plyr__controls .plyr__controls__item.plyr__time:first-child {
  padding-left: 0;
}
.plyr__controls .plyr__controls__item.plyr__volume {
  padding-right: 5px;
}
.plyr__controls .plyr__controls__item.plyr__volume:first-child {
  padding-right: 0;
}
.plyr__controls:empty {
  display: none;
}
.plyr--audio .plyr__controls {
  background: #fff;
  border-radius: inherit;
  color: #4f5b5f;
  padding: 10px;
}
.plyr--video .plyr__controls {
  background: linear-gradient(rgba(0, 0, 0, 0), rgba(0, 0, 0, 0.7));
  border-bottom-left-radius: inherit;
  border-bottom-right-radius: inherit;
  bottom: 0;
  color: #fff;
  left: 0;
  padding: 20px 5px 5px;
  position: absolute;
  right: 0;
  transition: opacity 0.4s ease-in-out, transform 0.4s ease-in-out;
  z-index: 3;
}
@media (min-width: 480px) {
  .plyr--video .plyr__controls {
    padding: 35px 10px 10px;
  }
}
.plyr--video.plyr--hide-controls .plyr__controls {
  opacity: 0;
  pointer-events: none;
  transform: translateY(100%);
}
.plyr [data-plyr="airplay"],
.plyr [data-plyr="captions"],
.plyr [data-plyr="fullscreen"],
.plyr [data-plyr="pip"] {
  display: none;
}
.plyr--airplay-supported [data-plyr="airplay"],
.plyr--captions-enabled [data-plyr="captions"],
.plyr--fullscreen-enabled [data-plyr="fullscreen"],
.plyr--pip-supported [data-plyr="pip"] {
  display: inline-block;
}
.plyr__menu {
  display: flex;
  position: relative;
}
.plyr__menu .plyr__control svg {
  transition: transform 0.3s ease;
}
.plyr__menu .plyr__control[aria-expanded="true"] svg {
  transform: rotate(90deg);
}
.plyr__menu .plyr__control[aria-expanded="true"] .plyr__tooltip {
  display: none;
}
.plyr__menu__container {
  animation: plyr-popup 0.2s ease;
  background: rgba(255, 255, 255, 0.9);
  border-radius: 4px;
  bottom: 100%;
  box-shadow: 0 1px 2px rgba(0, 0, 0, 0.15);
  color: #4f5b5f;
  font-size: 16px;
  margin-bottom: 10px;
  position: absolute;
  right: -3px;
  text-align: left;
  white-space: nowrap;
  z-index: 3;
}
.plyr__menu__container > div {
  overflow: hidden;
  transition: height 0.35s cubic-bezier(0.4, 0, 0.2, 1),
    width 0.35s cubic-bezier(0.4, 0, 0.2, 1);
}
.plyr__menu__container::after {
  border: 4px solid transparent;
  border-top-color: rgba(255, 255, 255, 0.9);
  content: "";
  height: 0;
  position: absolute;
  right: 15px;
  top: 100%;
  width: 0;
}
.plyr__menu__container [role="menu"] {
  padding: 7px;
}
.plyr__menu__container [role="menuitem"],
.plyr__menu__container [role="menuitemradio"] {
  margin-top: 2px;
}
.plyr__menu__container [role="menuitem"]:first-child,
.plyr__menu__container [role="menuitemradio"]:first-child {
  margin-top: 0;
}
.plyr__menu__container .plyr__control {
  align-items: center;
  color: #4f5b5f;
  display: flex;
  font-size: 14px;
  padding: 4px 11px;
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
  width: 100%;
}
.plyr__menu__container .plyr__control > span {
  align-items: inherit;
  display: flex;
  width: 100%;
}
.plyr__menu__container .plyr__control::after {
  border: 4px solid transparent;
  content: "";
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
}
.plyr__menu__container .plyr__control--forward {
  padding-right: 28px;
}
.plyr__menu__container .plyr__control--forward::after {
  border-left-color: rgba(79, 91, 95, 0.8);
  right: 5px;
}
.plyr__menu__container .plyr__control--forward.plyr__tab-focus::after,
.plyr__menu__container .plyr__control--forward:hover::after {
  border-left-color: currentColor;
}
.plyr__menu__container .plyr__control--back {
  font-weight: 500;
  margin: 7px;
  margin-bottom: 3px;
  padding-left: 28px;
  position: relative;
  width: calc(100% - 14px);
}
.plyr__menu__container .plyr__control--back::after {
  border-right-color: rgba(79, 91, 95, 0.8);
  left: 7px;
}
.plyr__menu__container .plyr__control--back::before {
  background: #b7c5cd;
  box-shadow: 0 1px 0 #fff;
  content: "";
  height: 1px;
  left: 0;
  margin-top: 4px;
  overflow: hidden;
  position: absolute;
  right: 0;
  top: 100%;
}
.plyr__menu__container .plyr__control--back.plyr__tab-focus::after,
.plyr__menu__container .plyr__control--back:hover::after {
  border-right-color: currentColor;
}
.plyr__menu__container .plyr__control[role="menuitemradio"] {
  padding-left: 7px;
}
.plyr__menu__container .plyr__control[role="menuitemradio"]::after,
.plyr__menu__container .plyr__control[role="menuitemradio"]::before {
  border-radius: 100%;
}
.plyr__menu__container .plyr__control[role="menuitemradio"]::before {
  background: rgba(0, 0, 0, 0.1);
  content: "";
  display: block;
  flex-shrink: 0;
  height: 16px;
  margin-right: 10px;
  transition: all 0.3s ease;
  width: 16px;
}
.plyr__menu__container .plyr__control[role="menuitemradio"]::after {
  background: #fff;
  border: 0;
  height: 6px;
  left: 12px;
  opacity: 0;
  top: 50%;
  transform: translateY(-50%) scale(0);
  transition: transform 0.3s ease, opacity 0.3s ease;
  width: 6px;
}
.plyr__menu__container
  .plyr__control[role="menuitemradio"][aria-checked="true"]::before {
  background: #1aafff;
}
.plyr__menu__container
  .plyr__control[role="menuitemradio"][aria-checked="true"]::after {
  opacity: 1;
  transform: translateY(-50%) scale(1);
}
.plyr__menu__container
  .plyr__control[role="menuitemradio"].plyr__tab-focus::before,
.plyr__menu__container .plyr__control[role="menuitemradio"]:hover::before {
  background: rgba(0, 0, 0, 0.1);
}
.plyr__menu__container .plyr__menu__value {
  align-items: center;
  display: flex;
  margin-left: auto;
  margin-right: -5px;
  overflow: hidden;
  padding-left: 25px;
  pointer-events: none;
}
.plyr--full-ui input[type="range"] {
  -webkit-appearance: none;
  background: 0 0;
  border: 0;
  border-radius: 26px;
  color: #1aafff;
  display: block;
  height: 19px;
  margin: 0;
  padding: 0;
  transition: box-shadow 0.3s ease;
  width: 100%;
}
.plyr--full-ui input[type="range"]::-webkit-slider-runnable-track {
  background: 0 0;
  border: 0;
  border-radius: 2.5px;
  height: 5px;
  transition: box-shadow 0.3s ease;
  -webkit-user-select: none;
  user-select: none;
  background-image: linear-gradient(
    to right,
    currentColor var(--value, 0),
    transparent var(--value, 0)
  );
}
.plyr--full-ui input[type="range"]::-webkit-slider-thumb {
  background: #fff;
  border: 0;
  border-radius: 100%;
  box-shadow: 0 1px 1px rgba(0, 0, 0, 0.15), 0 0 0 1px rgba(47, 52, 61, 0.2);
  height: 13px;
  position: relative;
  transition: all 0.2s ease;
  width: 13px;
  -webkit-appearance: none;
  margin-top: -4px;
}
.plyr--full-ui input[type="range"]::-moz-range-track {
  background: 0 0;
  border: 0;
  border-radius: 2.5px;
  height: 5px;
  transition: box-shadow 0.3s ease;
  -moz-user-select: none;
  user-select: none;
}
.plyr--full-ui input[type="range"]::-moz-range-thumb {
  background: #fff;
  border: 0;
  border-radius: 100%;
  box-shadow: 0 1px 1px rgba(0, 0, 0, 0.15), 0 0 0 1px rgba(47, 52, 61, 0.2);
  height: 13px;
  position: relative;
  transition: all 0.2s ease;
  width: 13px;
}
.plyr--full-ui input[type="range"]::-moz-range-progress {
  background: currentColor;
  border-radius: 2.5px;
  height: 5px;
}
.plyr--full-ui input[type="range"]::-ms-track {
  background: 0 0;
  border: 0;
  border-radius: 2.5px;
  height: 5px;
  transition: box-shadow 0.3s ease;
  -ms-user-select: none;
  user-select: none;
  color: transparent;
}
.plyr--full-ui input[type="range"]::-ms-fill-upper {
  background: 0 0;
  border: 0;
  border-radius: 2.5px;
  height: 5px;
  transition: box-shadow 0.3s ease;
  -ms-user-select: none;
  user-select: none;
}
.plyr--full-ui input[type="range"]::-ms-fill-lower {
  background: 0 0;
  border: 0;
  border-radius: 2.5px;
  height: 5px;
  transition: box-shadow 0.3s ease;
  -ms-user-select: none;
  user-select: none;
  background: currentColor;
}
.plyr--full-ui input[type="range"]::-ms-thumb {
  background: #fff;
  border: 0;
  border-radius: 100%;
  box-shadow: 0 1px 1px rgba(0, 0, 0, 0.15), 0 0 0 1px rgba(47, 52, 61, 0.2);
  height: 13px;
  position: relative;
  transition: all 0.2s ease;
  width: 13px;
  margin-top: 0;
}
.plyr--full-ui input[type="range"]::-ms-tooltip {
  display: none;
}
.plyr--full-ui input[type="range"]:focus {
  outline: 0;
}
.plyr--full-ui input[type="range"]::-moz-focus-outer {
  border: 0;
}
.plyr--full-ui
  input[type="range"].plyr__tab-focus::-webkit-slider-runnable-track {
  box-shadow: 0 0 0 5px rgba(26, 175, 255, 0.5);
  outline: 0;
}
.plyr--full-ui input[type="range"].plyr__tab-focus::-moz-range-track {
  box-shadow: 0 0 0 5px rgba(26, 175, 255, 0.5);
  outline: 0;
}
.plyr--full-ui input[type="range"].plyr__tab-focus::-ms-track {
  box-shadow: 0 0 0 5px rgba(26, 175, 255, 0.5);
  outline: 0;
}
.plyr--full-ui.plyr--video input[type="range"]::-webkit-slider-runnable-track {
  background-color: rgba(255, 255, 255, 0.25);
}
.plyr--full-ui.plyr--video input[type="range"]::-moz-range-track {
  background-color: rgba(255, 255, 255, 0.25);
}
.plyr--full-ui.plyr--video input[type="range"]::-ms-track {
  background-color: rgba(255, 255, 255, 0.25);
}
.plyr--full-ui.plyr--video input[type="range"]:active::-webkit-slider-thumb {
  box-shadow: 0 1px 1px rgba(0, 0, 0, 0.15), 0 0 0 1px rgba(47, 52, 61, 0.2),
    0 0 0 3px rgba(255, 255, 255, 0.5);
}
.plyr--full-ui.plyr--video input[type="range"]:active::-moz-range-thumb {
  box-shadow: 0 1px 1px rgba(0, 0, 0, 0.15), 0 0 0 1px rgba(47, 52, 61, 0.2),
    0 0 0 3px rgba(255, 255, 255, 0.5);
}
.plyr--full-ui.plyr--video input[type="range"]:active::-ms-thumb {
  box-shadow: 0 1px 1px rgba(0, 0, 0, 0.15), 0 0 0 1px rgba(47, 52, 61, 0.2),
    0 0 0 3px rgba(255, 255, 255, 0.5);
}
.plyr--full-ui.plyr--audio input[type="range"]::-webkit-slider-runnable-track {
  background-color: rgba(183, 197, 205, 0.66);
}
.plyr--full-ui.plyr--audio input[type="range"]::-moz-range-track {
  background-color: rgba(183, 197, 205, 0.66);
}
.plyr--full-ui.plyr--audio input[type="range"]::-ms-track {
  background-color: rgba(183, 197, 205, 0.66);
}
.plyr--full-ui.plyr--audio input[type="range"]:active::-webkit-slider-thumb {
  box-shadow: 0 1px 1px rgba(0, 0, 0, 0.15), 0 0 0 1px rgba(47, 52, 61, 0.2),
    0 0 0 3px rgba(0, 0, 0, 0.1);
}
.plyr--full-ui.plyr--audio input[type="range"]:active::-moz-range-thumb {
  box-shadow: 0 1px 1px rgba(0, 0, 0, 0.15), 0 0 0 1px rgba(47, 52, 61, 0.2),
    0 0 0 3px rgba(0, 0, 0, 0.1);
}
.plyr--full-ui.plyr--audio input[type="range"]:active::-ms-thumb {
  box-shadow: 0 1px 1px rgba(0, 0, 0, 0.15), 0 0 0 1px rgba(47, 52, 61, 0.2),
    0 0 0 3px rgba(0, 0, 0, 0.1);
}
.plyr__poster {
  background-color: #000;
  background-position: 50% 50%;
  background-repeat: no-repeat;
  background-size: contain;
  height: 100%;
  left: 0;
  opacity: 0;
  position: absolute;
  top: 0;
  transition: opacity 0.2s ease;
  width: 100%;
  z-index: 1;
}
.plyr--stopped.plyr__poster-enabled .plyr__poster {
  opacity: 1;
}
.plyr__time {
  font-size: 14px;
}
.plyr__time + .plyr__time::before {
  content: "\2044";
  margin-right: 10px;
}
@media (max-width: 767px) {
  .plyr__time + .plyr__time {
    display: none;
  }
}
.plyr--video .plyr__time {
  text-shadow: 0 1px 1px rgba(0, 0, 0, 0.15);
}
.plyr__tooltip {
  background: rgba(255, 255, 255, 0.9);
  border-radius: 3px;
  bottom: 100%;
  box-shadow: 0 1px 2px rgba(0, 0, 0, 0.15);
  color: #4f5b5f;
  font-size: 14px;
  font-weight: 500;
  left: 50%;
  line-height: 1.3;
  margin-bottom: 10px;
  opacity: 0;
  padding: 5px 7.5px;
  pointer-events: none;
  position: absolute;
  transform: translate(-50%, 10px) scale(0.8);
  transform-origin: 50% 100%;
  transition: transform 0.2s 0.1s ease, opacity 0.2s 0.1s ease;
  white-space: nowrap;
  z-index: 2;
}
.plyr__tooltip::before {
  border-left: 4px solid transparent;
  border-right: 4px solid transparent;
  border-top: 4px solid rgba(255, 255, 255, 0.9);
  bottom: -4px;
  content: "";
  height: 0;
  left: 50%;
  position: absolute;
  transform: translateX(-50%);
  width: 0;
  z-index: 2;
}
.plyr .plyr__control.plyr__tab-focus .plyr__tooltip,
.plyr .plyr__control:hover .plyr__tooltip,
.plyr__tooltip--visible {
  opacity: 1;
  transform: translate(-50%, 0) scale(1);
}
.plyr .plyr__control:hover .plyr__tooltip {
  z-index: 3;
}
.plyr__controls > .plyr__control:first-child .plyr__tooltip,
.plyr__controls > .plyr__control:first-child + .plyr__control .plyr__tooltip {
  left: 0;
  transform: translate(0, 10px) scale(0.8);
  transform-origin: 0 100%;
}
.plyr__controls > .plyr__control:first-child .plyr__tooltip::before,
.plyr__controls
  > .plyr__control:first-child
  + .plyr__control
  .plyr__tooltip::before {
  left: 16px;
}
.plyr__controls > .plyr__control:last-child .plyr__tooltip {
  left: auto;
  right: 0;
  transform: translate(0, 10px) scale(0.8);
  transform-origin: 100% 100%;
}
.plyr__controls > .plyr__control:last-child .plyr__tooltip::before {
  left: auto;
  right: 16px;
  transform: translateX(50%);
}
.plyr__controls > .plyr__control:first-child .plyr__tooltip--visible,
.plyr__controls
  > .plyr__control:first-child
  + .plyr__control
  .plyr__tooltip--visible,
.plyr__controls
  > .plyr__control:first-child
  + .plyr__control.plyr__tab-focus
  .plyr__tooltip,
.plyr__controls
  > .plyr__control:first-child
  + .plyr__control:hover
  .plyr__tooltip,
.plyr__controls > .plyr__control:first-child.plyr__tab-focus .plyr__tooltip,
.plyr__controls > .plyr__control:first-child:hover .plyr__tooltip,
.plyr__controls > .plyr__control:last-child .plyr__tooltip--visible,
.plyr__controls > .plyr__control:last-child.plyr__tab-focus .plyr__tooltip,
.plyr__controls > .plyr__control:last-child:hover .plyr__tooltip {
  transform: translate(0, 0) scale(1);
}
.plyr--video {
  background: #000;
  overflow: hidden;
}
.plyr--video.plyr--menu-open {
  overflow: visible;
}
.plyr__video-wrapper {
  background: #000;
  border-radius: inherit;
  overflow: hidden;
  position: relative;
  z-index: 0;
}
.plyr__video-embed,
.plyr__video-wrapper--fixed-ratio {
  height: 0;
  padding-bottom: 56.25%;
}
.plyr__video-embed iframe,
.plyr__video-wrapper--fixed-ratio video {
  border: 0;
  height: 100%;
  left: 0;
  position: absolute;
  top: 0;
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
  width: 100%;
}
.plyr--full-ui .plyr__video-embed > .plyr__video-embed__container {
  padding-bottom: 240%;
  position: relative;
  transform: translateY(-38.28125%);
}
.plyr__progress {
  left: 6.5px;
  margin-right: 80px;
  position: relative;
}
.plyr__progress input[type="range"],
.plyr__progress__buffer {
  margin-left: -6.5px;
  margin-right: -6.5px;
  width: calc(100% + 13px);
}
.plyr__progress input[type="range"] {
  position: relative;
  z-index: 2;
}
.plyr__progress .plyr__tooltip {
  font-size: 14px;
  left: 0;
}
.plyr__progress__buffer {
  -webkit-appearance: none;
  background: 0 0;
  border: 0;
  border-radius: 100px;
  height: 5px;
  left: 0;
  margin-top: -2.5px;
  padding: 0;
  position: absolute;
  top: 50%;
}
.plyr__progress__buffer::-webkit-progress-bar {
  background: 0 0;
}
.plyr__progress__buffer::-webkit-progress-value {
  background: currentColor;
  border-radius: 100px;
  min-width: 5px;
  transition: width 0.2s ease;
}
.plyr__progress__buffer::-moz-progress-bar {
  background: currentColor;
  border-radius: 100px;
  min-width: 5px;
  transition: width 0.2s ease;
}
.plyr__progress__buffer::-ms-fill {
  border-radius: 100px;
  transition: width 0.2s ease;
}
.plyr--video .plyr__progress__buffer {
  box-shadow: 0 1px 1px rgba(0, 0, 0, 0.15);
  color: rgba(255, 255, 255, 0.25);
}
.plyr--audio .plyr__progress__buffer {
  color: rgba(183, 197, 205, 0.66);
}
.plyr--loading .plyr__progress__buffer {
  animation: plyr-progress 1s linear infinite;
  background-image: linear-gradient(
    -45deg,
    rgba(47, 52, 61, 0.6) 25%,
    transparent 25%,
    transparent 50%,
    rgba(47, 52, 61, 0.6) 50%,
    rgba(47, 52, 61, 0.6) 75%,
    transparent 75%,
    transparent
  );
  background-repeat: repeat-x;
  background-size: 25px 25px;
  color: transparent;
}
.plyr--video.plyr--loading .plyr__progress__buffer {
  background-color: rgba(255, 255, 255, 0.25);
}
.plyr--audio.plyr--loading .plyr__progress__buffer {
  background-color: rgba(183, 197, 205, 0.66);
}
.plyr__volume {
  align-items: center;
  display: flex;
  flex: 1;
  position: relative;
}
.plyr__volume input[type="range"] {
  margin-left: 5px;
  position: relative;
  z-index: 2;
}
@media (min-width: 480px) {
  .plyr__volume {
    max-width: 90px;
  }
}
@media (min-width: 768px) {
  .plyr__volume {
    max-width: 110px;
  }
}
.plyr--is-ios .plyr__volume {
  display: none !important;
}
.plyr--is-ios.plyr--vimeo [data-plyr="mute"] {
  display: none !important;
}
.plyr:-webkit-full-screen {
  background: #000;
  border-radius: 0 !important;
  height: 100%;
  margin: 0;
  width: 100%;
}
.plyr:-ms-fullscreen {
  background: #000;
  border-radius: 0 !important;
  height: 100%;
  margin: 0;
  width: 100%;
}
.plyr:fullscreen {
  background: #000;
  border-radius: 0 !important;
  height: 100%;
  margin: 0;
  width: 100%;
}
.plyr:-webkit-full-screen video {
  height: 100%;
}
.plyr:-ms-fullscreen video {
  height: 100%;
}
.plyr:fullscreen video {
  height: 100%;
}
.plyr:-webkit-full-screen .plyr__video-wrapper {
  height: 100%;
  width: 100%;
}
.plyr:-ms-fullscreen .plyr__video-wrapper {
  height: 100%;
  width: 100%;
}
.plyr:fullscreen .plyr__video-wrapper {
  height: 100%;
  width: 100%;
}
.plyr:-webkit-full-screen.plyr--vimeo .plyr__video-wrapper {
  height: 0;
  top: 50%;
  transform: translateY(-50%);
}
.plyr:-ms-fullscreen.plyr--vimeo .plyr__video-wrapper {
  height: 0;
  top: 50%;
  transform: translateY(-50%);
}
.plyr:fullscreen.plyr--vimeo .plyr__video-wrapper {
  height: 0;
  top: 50%;
  transform: translateY(-50%);
}
.plyr:-webkit-full-screen .plyr__control .icon--exit-fullscreen {
  display: block;
}
.plyr:-ms-fullscreen .plyr__control .icon--exit-fullscreen {
  display: block;
}
.plyr:fullscreen .plyr__control .icon--exit-fullscreen {
  display: block;
}
.plyr:-webkit-full-screen .plyr__control .icon--exit-fullscreen + svg {
  display: none;
}
.plyr:-ms-fullscreen .plyr__control .icon--exit-fullscreen + svg {
  display: none;
}
.plyr:fullscreen .plyr__control .icon--exit-fullscreen + svg {
  display: none;
}
.plyr:-webkit-full-screen.plyr--hide-controls {
  cursor: none;
}
.plyr:-ms-fullscreen.plyr--hide-controls {
  cursor: none;
}
.plyr:fullscreen.plyr--hide-controls {
  cursor: none;
}
@media (min-width: 1024px) {
  .plyr:-webkit-full-screen .plyr__captions {
    font-size: 21px;
  }
  .plyr:-ms-fullscreen .plyr__captions {
    font-size: 21px;
  }
  .plyr:fullscreen .plyr__captions {
    font-size: 21px;
  }
}
.plyr:-webkit-full-screen {
  background: #000;
  border-radius: 0 !important;
  height: 100%;
  margin: 0;
  width: 100%;
}
.plyr:-webkit-full-screen video {
  height: 100%;
}
.plyr:-webkit-full-screen .plyr__video-wrapper {
  height: 100%;
  width: 100%;
}
.plyr:-webkit-full-screen.plyr--vimeo .plyr__video-wrapper {
  height: 0;
  top: 50%;
  transform: translateY(-50%);
}
.plyr:-webkit-full-screen .plyr__control .icon--exit-fullscreen {
  display: block;
}
.plyr:-webkit-full-screen .plyr__control .icon--exit-fullscreen + svg {
  display: none;
}
.plyr:-webkit-full-screen.plyr--hide-controls {
  cursor: none;
}
@media (min-width: 1024px) {
  .plyr:-webkit-full-screen .plyr__captions {
    font-size: 21px;
  }
}
.plyr:-moz-full-screen {
  background: #000;
  border-radius: 0 !important;
  height: 100%;
  margin: 0;
  width: 100%;
}
.plyr:-moz-full-screen video {
  height: 100%;
}
.plyr:-moz-full-screen .plyr__video-wrapper {
  height: 100%;
  width: 100%;
}
.plyr:-moz-full-screen.plyr--vimeo .plyr__video-wrapper {
  height: 0;
  top: 50%;
  transform: translateY(-50%);
}
.plyr:-moz-full-screen .plyr__control .icon--exit-fullscreen {
  display: block;
}
.plyr:-moz-full-screen .plyr__control .icon--exit-fullscreen + svg {
  display: none;
}
.plyr:-moz-full-screen.plyr--hide-controls {
  cursor: none;
}
@media (min-width: 1024px) {
  .plyr:-moz-full-screen .plyr__captions {
    font-size: 21px;
  }
}
.plyr:-ms-fullscreen {
  background: #000;
  border-radius: 0 !important;
  height: 100%;
  margin: 0;
  width: 100%;
}
.plyr:-ms-fullscreen video {
  height: 100%;
}
.plyr:-ms-fullscreen .plyr__video-wrapper {
  height: 100%;
  width: 100%;
}
.plyr:-ms-fullscreen.plyr--vimeo .plyr__video-wrapper {
  height: 0;
  top: 50%;
  transform: translateY(-50%);
}
.plyr:-ms-fullscreen .plyr__control .icon--exit-fullscreen {
  display: block;
}
.plyr:-ms-fullscreen .plyr__control .icon--exit-fullscreen + svg {
  display: none;
}
.plyr:-ms-fullscreen.plyr--hide-controls {
  cursor: none;
}
@media (min-width: 1024px) {
  .plyr:-ms-fullscreen .plyr__captions {
    font-size: 21px;
  }
}
.plyr--fullscreen-fallback {
  background: #000;
  border-radius: 0 !important;
  height: 100%;
  margin: 0;
  width: 100%;
  bottom: 0;
  left: 0;
  position: fixed;
  right: 0;
  top: 0;
  z-index: 10000000;
}
.plyr--fullscreen-fallback video {
  height: 100%;
}
.plyr--fullscreen-fallback .plyr__video-wrapper {
  height: 100%;
  width: 100%;
}
.plyr--fullscreen-fallback.plyr--vimeo .plyr__video-wrapper {
  height: 0;
  top: 50%;
  transform: translateY(-50%);
}
.plyr--fullscreen-fallback .plyr__control .icon--exit-fullscreen {
  display: block;
}
.plyr--fullscreen-fallback .plyr__control .icon--exit-fullscreen + svg {
  display: none;
}
.plyr--fullscreen-fallback.plyr--hide-controls {
  cursor: none;
}
@media (min-width: 1024px) {
  .plyr--fullscreen-fallback .plyr__captions {
    font-size: 21px;
  }
}
.plyr__ads {
  border-radius: inherit;
  bottom: 0;
  cursor: pointer;
  left: 0;
  overflow: hidden;
  position: absolute;
  right: 0;
  top: 0;
  z-index: -1;
}
.plyr__ads > div,
.plyr__ads > div iframe {
  height: 100%;
  position: absolute;
  width: 100%;
}
.plyr__ads::after {
  background: rgba(47, 52, 61, 0.8);
  border-radius: 2px;
  bottom: 10px;
  color: #fff;
  content: attr(data-badge-text);
  font-size: 11px;
  padding: 2px 6px;
  pointer-events: none;
  position: absolute;
  right: 10px;
  z-index: 3;
}
.plyr__ads::after:empty {
  display: none;
}
.plyr__cues {
  background: currentColor;
  display: block;
  height: 5px;
  left: 0;
  margin: -2.5px 0 0;
  opacity: 0.8;
  position: absolute;
  top: 50%;
  width: 3px;
  z-index: 3;
}
.plyr__preview-thumb {
  background-color: rgba(255, 255, 255, 0.9);
  border-radius: 3px;
  bottom: 100%;
  box-shadow: 0 1px 2px rgba(0, 0, 0, 0.15);
  margin-bottom: 10px;
  opacity: 0;
  padding: 3px;
  pointer-events: none;
  position: absolute;
  transform: translate(0, 10px) scale(0.8);
  transform-origin: 50% 100%;
  transition: transform 0.2s 0.1s ease, opacity 0.2s 0.1s ease;
  z-index: 2;
}
.plyr__preview-thumb--is-shown {
  opacity: 1;
  transform: translate(0, 0) scale(1);
}
.plyr__preview-thumb::before {
  border-left: 4px solid transparent;
  border-right: 4px solid transparent;
  border-top: 4px solid rgba(255, 255, 255, 0.9);
  bottom: -4px;
  content: "";
  height: 0;
  left: 50%;
  position: absolute;
  transform: translateX(-50%);
  width: 0;
  z-index: 2;
}
.plyr__preview-thumb__image-container {
  background: #b7c5cd;
  border-radius: 2px;
  overflow: hidden;
  position: relative;
  z-index: 0;
}
.plyr__preview-thumb__image-container img {
  height: 100%;
  left: 0;
  max-height: none;
  max-width: none;
  position: absolute;
  top: 0;
  width: 100%;
}
.plyr__preview-thumb__time-container {
  bottom: 6px;
  left: 0;
  position: absolute;
  right: 0;
  white-space: nowrap;
  z-index: 3;
}
.plyr__preview-thumb__time-container span {
  background-color: rgba(0, 0, 0, 0.55);
  border-radius: 2px;
  color: #fff;
  font-size: 14px;
  padding: 3px 6px;
}
.plyr__preview-scrubbing {
  bottom: 0;
  filter: blur(1px);
  height: 100%;
  left: 0;
  margin: auto;
  opacity: 0;
  overflow: hidden;
  position: absolute;
  right: 0;
  top: 0;
  transition: opacity 0.3s ease;
  width: 100%;
  z-index: 1;
}
.plyr__preview-scrubbing--is-shown {
  opacity: 1;
}
.plyr__preview-scrubbing img {
  height: 100%;
  left: 0;
  max-height: none;
  max-width: none;
  -o-object-fit: contain;
  object-fit: contain;
  position: absolute;
  top: 0;
  width: 100%;
}
.plyr--no-transition {
  transition: none !important;
}
.plyr__sr-only {
  clip: rect(1px, 1px, 1px, 1px);
  overflow: hidden;
  border: 0 !important;
  height: 1px !important;
  padding: 0 !important;
  position: absolute !important;
  width: 1px !important;
}
.plyr [hidden] {
  display: none !important;
}
</style>
