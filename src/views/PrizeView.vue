<script>
export default {
  methods: {
    async homePage() {
      this.loader = true // Show the loader
      this.$router.push({ path: '/' }) // Navigate to /quizme
    },
    playAudio() {
      const audio = this.$refs.bgmusic
      const video = this.$refs.video
      if (video) {
        video.play().catch((error) => {
          console.error('Video autoplay failed:', error)
        })
      }
      if (audio) {
        audio.volume = 0.5 // Set volume to 50%
        audio.play().catch((error) => {
          console.error('Playback failed:', error)
        })
      }
    },
  },
}
</script>

<template>
  <div class="bg-red-500">
    <audio ref="bgmusic" loop>
      <source src="/public/bgmusic8d.mp3" />
    </audio>
    <div class="paper">
      <div class="flex justify-center items-center mt-10">
        <button
          @click="playAudio"
          class="jua-regular cursor-pointer hover:scale-105 transition-all duration-300 hover:underline"
        >
          Click me to play
        </button>
      </div>

      <div class="lines">
        <div class="text" contenteditable spellcheck="false">
          Hi, lovee <br /><br />
          I just wanna say congratulations and ang galling mo palagi hindi ko man always nakikita I
          know na you always doing your best para makasurvive not only academically but also in
          life. I know you have battles in life na mas mabigat pa sa dinadala mo ngayon, but I'm
          always here praying and cheering for you. Lagi mong iovercome 'yan haa Kahit anong
          mangyari I'm always proud sa mga na achieve mo at maachieve mo. "I may not always find the
          right words, but when I write for someone, it’s always genuine, sorry sa mga times na
          minsan wala ako and alam kong naiintindihan mo rin yun, sana makita ko na si coco jk
          HAHAHAHAHA. Ayun lang i love you sheeyy, Happy Monthsary poo! ❤ ❤ ❤ <br />
          <div class="flex justify-end">- Aiii</div>

          <div class="flex flex-col gap-3 justify-center items-center">
            <video ref="video" src="/public/me.mp4" muted autoplay width="100" height="200"></video>
            <label>This is me while doing this app hehehe</label>
            <button
              @click="homePage"
              class="jua-regular cursor-pointer hover:scale-105 transition-all duration-300 hover:underline hover:text-red-500"
            >
              Home
            </button>
          </div>
        </div>
      </div>
      <div class="holes hole-top"></div>
      <div class="holes hole-middle"></div>
      <div class="holes hole-bottom"></div>
    </div>
  </div>
</template>
<style>
@import url(https://fonts.googleapis.com/css?family=Indie+Flower);
body {
  margin: 0;
  padding: 0;
  background: lightgoldenrodyellow;
}
.paper {
  position: absolute;
  height: 550px;
  width: 450px;
  background: rgba(255, 255, 255, 0.9);
  margin: -275px -225px;
  left: 50%;
  top: 50%;
  box-shadow: 0px 0px 5px 0px #888;
}
.paper::before {
  content: '';
  position: absolute;
  left: 45px;
  height: 100%;
  width: 2px;
  background: rgba(255, 0, 0, 0.4);
}
.lines {
  margin-top: 40px;
  height: calc(100% - 40px);
  width: 100%;
  background-image: repeating-linear-gradient(white 0px, white 24px, steelblue 25px);
}
.text {
  position: absolute;
  top: 65px;
  left: 55px;
  bottom: 10px;
  right: 10px;
  line-height: 25px;
  font-family: 'Indie Flower';
  overflow: hidden;
  outline: none;
}
.holes {
  position: absolute;
  left: 10px;
  height: 25px;
  width: 25px;
  background: lightgoldenrodyellow;
  border-radius: 50%;
  box-shadow: inset 0px 0px 2px 0px #888;
}
.hole-top {
  top: 10%;
}
.hole-middle {
  top: 50%;
}
.hole-bottom {
  bottom: 10%;
}
</style>
