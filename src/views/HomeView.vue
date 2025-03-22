<script>
import Loader from '../components/Loader.vue'

export default {
  components: {
    Loader,
  },
  data() {
    return {
      loader: false,
    }
  },
  methods: {
    playClickSound() {
      const audio = this.$refs.clickAudio;
      if (audio) {
        try {
          audio.currentTime = 0; // Reset the audio to the start
          audio.play(); // Play the audio
        } catch (error) {
          console.error('Audio playback failed:', error);
        }
      } else {
        console.error('Audio element not found');
      }
    },
    async navigateToQuiz() {
      this.playClickSound(); // Play the click sound
      this.loader = true; // Show the loader
      await new Promise((resolve) => setTimeout(resolve, 3000)); // 3-second delay
      this.$router.push({ path: '/quizme' }); // Navigate to /quizme
    },
  },
}
</script>

<template>
  <main class="flex flex-col p-2 items-center">
    <!-- Keep the audio element outside the conditional rendering -->
    <audio ref="clickAudio" src="/public/click.mp3"></audio>
    
    <Loader v-if="loader" ref="loader" />
    <div v-else class="flex flex-col gap-5">
      <h1 class="jua-regular text-6xl text-white">Hello, I'm Ai</h1>
      <h3 class="text-3xl bubblegum-sans-regular text-white">do you want to know me?</h3>
      <!-- <TheWelcome /> -->
      <button
        @click="navigateToQuiz"
        class="p-2 text-white border hover:bg-skin-300 hover:scale-105 w-36 bubblegum-sans-regular cursor-pointer transition-all duration-300 border-white rounded-md shadow-xl"
      >
        Yes, please
      </button>
    </div>
  </main>
</template>