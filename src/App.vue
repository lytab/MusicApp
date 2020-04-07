<template>
<div id="app">
    <header>
        <h1>My Music</h1>
    </header>
    <main>
        <section class="player">
            <h2 class="song-title">{{current.title}} - <span>{{current.artist}}</span></h2>
            <div class="controls">
                <button class="prev" @click="prevSong">Prev</button>
                <button class="play" v-if="!isPlaying" @click="playSong">Play</button>
                <button class="pause" v-else @click="pauseSong">Pause</button>
                <button class="next" @click="nextSong">Next</button>
            </div>
        </section>
        <section class="playlist">
            <h3>The Palylist</h3>
            <button :class="song.src==current.src?'song playing':'song'" v-for="song in songs" :key="song.src" @click="playSong(song)">
                {{song.title}} - {{song.artist}}
            </button>
        </section>
    </main>
</div>
</template>

<script>
export default {
    name: 'App',
    data() {
        return {
            current: {},
            index: 0,
            isPlaying: false,
            songs: [
                { title: "Volare", artist: "Gipsy King", src: require('./assets/Gipsy Kings - Volare.mp3') },
                { title: "Bamboleo", artist: "Gipsy King", src: require('./assets/Gipsy Kings-Bamboleo HQ 1080.wmv.mp3') },
                { title: "Let Me Love You", artist: "DJ Snake ft. Justin Bieber", src: require('./assets/DJ Snake - Let Me Love You ft. Justin Bieber (320  kbps) (TubeMp3Convert.com).mp3') },
                { title: "Shape of You", artist: "Ed Sheeran", src: require('./assets/Ed Sheeran - Shape of You [Official Video] (320  kbps) (TubeMp3Convert.com).mp3') },
                { title: "Ya Lili", artist: "Balti feat. Hamouda", src: require('./assets/Balti - Ya Lili feat. Hamouda (Official Music Video) (320  kbps) (TubeMp3Convert.com).mp3') },
            ],
            player: new Audio(),
        }
    },
    created() {
        this.current = this.songs.length > this.index ? this.songs[this.index] : null;
        this.player.src = this.current.src;
        //this.player.play();
    },
    methods: {
        playSong(song) {
            if (typeof song.src != "undefined") {
                this.current = song;
                this.player.src = this.current.src;
            }
            this.player.play();
            this.player.addEventListener('ended', function () {
                this.nextSong();
            })
            this.isPlaying = true;
            this.index=this.songs.indexOf(this.current);
        },
        pauseSong() {
            this.player.pause();
            this.isPlaying = false;
        },
        nextSong() {
            this.index++;
            if (this.index > this.songs.length - 1) {
                this.index = 0;
            }
            this.current = this.songs[this.index];
            this.playSong(this.current);

        },
        prevSong() {
            this.index--;
            if (this.index < 0) {
                this.index = this.songs.length - 1;
            }
            this.current = this.songs[this.index];
            this.playSong(this.current);
        }
    },
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
    color: #fff;
}

main {
    width: 100%;
    max-width: 768px;
    margin: 0 auto;
    padding: 25px;
}

.song-title {
    color: #212121;
    font-size: 32px;
    font-weight: 700;
    text-transform: uppercase;
    text-align: center;
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
button{
  appearance: none;
  background: none;
border:none;
outline:none;
cursor: pointer;
}
button:hover{
opacity:0.8;
}
.play,.pause{
font-size: 20px;
font-weight: 700;
padding: 15px 25px;
margin:0px 15px;
border-radius: 8px;
color: #fff;
background-color: #cc2e5d;
}
.next,.prev{
font-size: 16px;
font-weight: 700;
padding: 10px 20px;
margin: 0px 15px;
border-radius: 6px;
color: #fff;
background-color: #ff5858;
}
.playlist{
  padding:0px 30px;

}
.playlist h3{
  color: #212121;
font-size:28px;
font-display: 400;
margin-bottom: 38px;
text-align: center;
}
.playlist .song{
 display:block;
width: 100%;
padding: 15px;
font-size: 28px;
font-weight: 700;
cursor: pointer;
}
.playlist .song:hover{
 color:#ff5858;

}
.playlist .song.playing{
color:#fff;
background-image:linear-gradient(to right,#cc2e5d,#ff5858);
}
</style>
