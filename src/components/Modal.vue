<script>
export default {
  data() {
    return {
      isVisible: false, // Visibility state of the modal
    }
  },
  methods: {
 
    openModal() {
      this.isVisible = true // Show modal
    },
    closeModal() {
      this.isVisible = false // Hide modal
    },
  },
  mounted() {
    // Expose openModal to the parent component
    this.$emit('expose', {
      openModal: this.openModal, // Expose openModal as a method
    })
  },
}
</script>

<template>
  <div
    v-if="isVisible"
    class="fixed inset-0 flex items-center justify-center bg-zinc-900/60 p-4 md:p-0 z-50"
  >
    <div
      class="bg-white border-2 border-orange-900 shadow-2xl p-6 rounded-lg relative scale-up-center md:w-1/2 flex justify-center flex-col items-center gap-5"
    >
      <!-- Modal content goes here -->
      <slot></slot>

      <!-- Close Button -->
      
    </div>
  </div>
</template>

<style></style>
