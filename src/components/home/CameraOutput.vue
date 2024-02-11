<template>
  <div class="min-h-screen flex flex-row justify-center items-center p-2 bg-[khaki]">
    <button
      v-if="scanButtonIsVisible"
      @click="startCamera"
      class="bg-[crimson] p-8 uppercase text-white text-2xl rounded-lg"
    >
      scan
    </button>
    <div v-if="scanButtonIsVisible === false">
      <video ref="video" autoplay></video>
      <button @click="takePhoto" class="bg-[crimson] p-2 mt-2 text-white rounded-lg">
        Take Photo
      </button>
      <img v-if="photo" :src="photo" alt="Captured Photo" class="w-32 mt-4" />
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const video = ref(null)
const photo = ref(null)
let scanButtonIsVisible = ref(true)

const constraints = {
  video: {
    facingMode: 'environment'
  }
}

const startCamera = async () => {
  scanButtonIsVisible.value = false
  try {
    const stream = await navigator.mediaDevices.getUserMedia(constraints)
    video.value.srcObject = stream
  } catch (err) {
    console.error('Error accessing the camera:', err)
  }
}

const takePhoto = () => {
  const canvas = document.createElement('canvas')
  canvas.width = video.value.videoWidth
  canvas.height = video.value.videoHeight
  canvas.getContext('2d').drawImage(video.value, 0, 0, canvas.width, canvas.height)
  photo.value = canvas.toDataURL('image/png')
}
</script>

<style scoped></style>
