<script>
import Modal from '../components/Modal.vue'
import Loader from '../components/Loader.vue'
import CountModal from '../components/CountModal.vue'
import Swal from 'sweetalert2'

export default {
  components: {
    Modal,
    Loader,
    CountModal,
  },
  data() {
    return {
      showQuestion: false,
      loader: false,
      progress: 100,
      questions: [
        { question: 'What is my full name?', answer: 'airon villaruel' },
        { question: 'What is my favorite color?', answer: 'blue' },
        { question: 'How old am I?', answer: '23' },
        { question: 'What is my favorite food', answer: 'sinigang na hipon' },
        { question: 'Am I handsome?', answer: 'yes' },
      ],
      timerInterval: null,
      currentQuestionIndex: 0,
      userAnswer: '',
      feedback: '',
      ModalRef: null,
      modalSuccess: null,
      modalCountdown: null,
      countdownValue: 3, // Add countdownValue to track the countdown
    }
  },
  methods: {
    tryAgain(message) {
      const Toast = Swal.mixin({
        toast: true,
        position: 'top-end',
        showConfirmButton: false,
        timer: 2000,
        didOpen: (toast) => {
          toast.onmouseenter = Swal.stopTimer
          toast.onmouseleave = Swal.resumeTimer
        },
      })
      Toast.fire({
        icon: 'error',
        title: message,
      })
    },

    async retryGame() {
      this.playClickSound() // Play the click sound

      window.location.reload()
    },
    async homePage() {
      this.playClickSound() // Play the click sound

      this.$refs.modalRef.closeModal()
      this.loader = true // Show the loader
      await new Promise((resolve) => setTimeout(resolve, 3000)) // 500ms delay
      this.$router.push({ path: '/' }) // Navigate to /quizme
    },
    async prizePage() {
      this.playClickSound() // Play the click sound

      this.$refs.modalSuccess.closeModal()
      this.loader = true // Show the loader
      await new Promise((resolve) => setTimeout(resolve, 3000)) // 500ms delay
      this.$router.push({ path: '/prize' }) // Navigate to /quizme
    },
    playClickSound() {
      const audio = this.$refs.clickAudio
      if (audio) {
        try {
          audio.currentTime = 0 // Reset the audio to the start
          audio.play() // Play the audio
        } catch (error) {
          console.error('Audio playback failed:', error)
        }
      } else {
        console.error('Audio element not found')
      }
    },
    playCountdown() {
      const audio = this.$refs.countDown
      if (audio) {
        try {
          audio.volume = 0.1
          audio.currentTime = 0 // Reset the audio to the start
          audio.play() // Play the audio
        } catch (error) {
          console.error('Audio playback failed:', error)
        }
      } else {
        console.error('Audio element not found')
      }
    },
    playTickTack() {
      const audio = this.$refs.tickTack
      if (audio) {
        try {
          audio.volume = 0.1
          audio.currentTime = 0 // Reset the audio to the start
          audio.play() // Play the audio
        } catch (error) {
          console.error('Audio playback failed:', error)
        }
      } else {
        console.error('Audio element not found')
      }
    },
    playGameOver() {
      const audio = this.$refs.gameOver
      if (audio) {
        try {
          audio.volume = 0.1
          audio.currentTime = 0 // Reset the audio to the start
          audio.play() // Play the audio
        } catch (error) {
          console.error('Audio playback failed:', error)
        }
      } else {
        console.error('Audio element not found')
      }
    },
    playGameSuccess() {
      const audio = this.$refs.gameSuccess
      if (audio) {
        try {
          audio.volume = 0.1
          audio.currentTime = 0 // Reset the audio to the start
          audio.play() // Play the audio
        } catch (error) {
          console.error('Audio playback failed:', error)
        }
      } else {
        console.error('Audio element not found')
      }
    },
    countdown() {
      this.timerInterval = setInterval(() => {
        if (this.progress > 0) {
          this.progress -= 100 / 30 // Decrease progress by 1/30th every second
        } else {
          clearInterval(this.timerInterval) // Stop the interval when progress reaches 0
          const audio = this.$refs.tickTack
          if (audio) {
            audio.pause()
            audio.currentTime = 0 // Reset the audio to the start
          }
          this.playGameOver() // Play the game over audio
          if (this.$refs.modalRef) {
            this.$refs.modalRef.openModal() // Call openModal method on the modal reference
          }
        }
      }, 1000) // Update every second
    },
    checkAnswer() {
      this.playClickSound() // Play the click sound
      const currentQuestion = this.questions[this.currentQuestionIndex]
      if (this.userAnswer.trim().toLowerCase() === currentQuestion.answer.toLowerCase()) {
        this.feedback = 'Correct!'
        this.currentQuestionIndex++
        this.userAnswer = ''
        if (this.currentQuestionIndex >= this.questions.length) {
          this.feedback = 'You completed the quiz!'
          clearInterval(this.timerInterval)
          if (this.$refs.modalSuccess) {
            this.playGameSuccess()
            const audio = this.$refs.tickTack
            if (audio) {
              audio.pause()
              audio.currentTime = 0 // Reset the audio to the start
            }
            this.$refs.modalSuccess.openModal() // Call openModal method on the modal reference
          }
        }
      } else {
        if (this.userAnswer === '') {
          this.tryAgain('Please enter your answer')
        } else if (this.userAnswer.trim().toLowerCase() === 'no') {
          this.tryAgain('Kupal kaba? boss?')
        } else {
          this.tryAgain('Incorrect! Try again.')
        }
      }
    },
    startCount() {
      if (this.$refs.modalCountdown) {
        this.$refs.modalCountdown.openModal() // Open the modal
        this.playCountdown() // Play the countdown audio immediately
        let countdownInterval = setInterval(() => {
          if (this.countdownValue > 1) {
            this.countdownValue-- // Decrement countdownValue
          } else {
            this.countdownValue = 'Start!' // Set to "Start!" when countdown ends
            clearInterval(countdownInterval) // Clear the interval
            setTimeout(() => {
              this.$refs.modalCountdown.closeModal() // Close the modal after a short delay
              this.playTickTack() // Play the tick-tack audio
              this.showQuestion = true
            }, 1000)
          }
        }, 1000) // Update every second
      }
    },
  },
  mounted() {
    // Start the countdown and decrease progress over 30 seconds
    this.startCount()
    this.countdown()
  },
}
</script>
<template>
  <div class="flex flex-col items-center justify-between p-2 w-full gap-4">
    <audio ref="clickAudio" src="/public/click.mp3"></audio>
    <audio ref="countDown" src="/public/count.mp3"></audio>
    <audio ref="tickTack" src="/public/tick.mp3" loop></audio>
    <audio ref="gameOver" src="/public/over.mp3"></audio>
    <audio ref="gameSuccess" src="/public/congratulations.mp3"></audio>

    <CountModal ref="modalCountdown">
      <div class="w-full flex flex-col gap-4 items-center p-2">
        <h1 class="text-5xl jua-regular text-white">{{ countdownValue }}</h1>
        <!-- Make the h1 dynamic -->
      </div>
    </CountModal>
    <Loader v-if="loader" ref="loader" />
    <Modal ref="modalRef">
      <div class="w-full flex flex-col gap-4 items-center p-2">
        <h1 class="jua-regular text-5xl">Try again?</h1>
        <div class="flex flex-row gap-4">
          <button
            class="p-2 hover:scale-105 transition-all duration-300 cursor-pointer"
            @click="homePage"
          >
            <img src="/public/back.png" class="w-12" />
          </button>
          <button
            class="p-2 hover:scale-105 transition-all duration-300 cursor-pointer"
            @click="retryGame"
          >
            <img src="/public/retry.png" class="w-12" />
          </button>
        </div>
      </div>
    </Modal>
    <Modal ref="modalSuccess">
      <div class="w-full flex flex-col gap-4 items-center p-2">
        <h1 class="jua-regular text-5xl">Congratulations!!</h1>
        <p class="jua-regular">
          <span class="text-red-500">Note:</span> Wear earphones for a better experience!
        </p>
        <button
          @click="prizePage"
          class="bg-peach p-2 text-2xl text-white rounded-xl shadow-xl bubblegum-sans-regular cursor-pointer hover:scale-105 transition-all duration-300"
        >
          Claim your prize!!
        </button>
        <div class="flex flex-row gap-4">
          <button
            class="p-2 hover:scale-105 transition-all duration-300 cursor-pointer"
            @click="homePage"
          >
            <img src="/public/back.png" class="w-12" />
          </button>
          <button
            class="p-2 hover:scale-105 transition-all duration-300 cursor-pointer"
            @click="retryGame"
          >
            <img src="/public/retry.png" class="w-12" />
          </button>
        </div>
      </div>
    </Modal>
    <div
      class="w-3/4 md:w-1/4 flex flex-row border-3 shadow-xl border-white rounded-xl p-2 gap-2 justify-center items-center"
    >
      <img src="/public/Stopwatch.png" class="w-10" alt="" />
      <div
        class="h-2 rounded-xl transition-all duration-1000"
        :style="{
          width: progress + '%',
          backgroundColor: progress > 0 ? 'white' : 'red',
        }"
      ></div>
    </div>
    <div
      class="w-3/4 md:w-1/2 flex flex-col bg-white p-4 items-center justify-center rounded-xl shadow-xl gap-2"
    >
      <h2 class="jua-regular text-4xl">Question</h2>
      <div class="p-2 w-1/2 break-words text-center">
        <p v-if="showQuestion" class="bubblegum-sans-regular text-xl">
          {{ questions[currentQuestionIndex]?.question || 'No more questions!' }}
        </p>
      </div>
      <div class="h-0.5 w-full bg-gray-200"></div>
      <div class="flex flex-col md:flex-row gap-4 items-center w-3/4 md:w-1/2">
        <label class="jua-regular text-xl">Answer:</label>
        <input
          type="text"
          v-model="userAnswer"
          @keydown.enter="checkAnswer"
          class="w-full p-1 border-2 jua-regular rounded-xl shadow-xl border-gray-200"
        />
        <button
          @click="checkAnswer"
          class="p-1 text-white border hover:bg-skin-200 bg-peach hover:scale-105 w-36 bubblegum-sans-regular cursor-pointer transition-all duration-300 border-white rounded-md shadow-xl"
        >
          Enter
        </button>
      </div>
    </div>
    <!-- <div class="w-1/4 bg-blue-500 p-2">Me</div> -->
  </div>
</template>
