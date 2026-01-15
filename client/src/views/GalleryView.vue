<script>
import { onBeforeUnmount, onMounted, ref } from 'vue'
import { useImages } from '../assets/useImages.js'
import AnimatedMacaroni from '../components/AnimatedMacaroni.vue'

export default {
  setup() {
    const { images } = useImages()
    const showModal = ref(false)
    const selectedImage = ref(null)

    const openModal = (index) => {
      selectedImage.value = index
      showModal.value = true
      setTimeout(() => {
        const closeBtn = document.querySelector('.modalPopUp .close')
        if (closeBtn) closeBtn.focus()
      }, 0)
    }

    const closeModal = () => {
      selectedImage.value = null
      showModal.value = false
    }

    const selectNext = () => {
      if (selectedImage.value < images.value.length - 1) {
        selectedImage.value += 1
      }
    }

    const selectPrev = () => {
      if (selectedImage.value > 0) {
        selectedImage.value -= 1
      }
    }

    const handleKeydown = (e) => {
      if (!showModal.value) return
      if (e.key === 'Escape') {
        closeModal()
      } else if (e.key === 'ArrowRight') {
        selectNext()
      } else if (e.key === 'ArrowLeft') {
        selectPrev()
      }
    }

    onMounted(() => {
      window.addEventListener('keydown', handleKeydown)
    })
    onBeforeUnmount(() => {
      window.removeEventListener('keydown', handleKeydown)
    })

    const handleImageKeydown = (e, idx) => {
      if (e.key === 'Enter' || e.key === ' ') {
        openModal(idx)
      }
    }

    return {
      images,
      showModal,
      selectedImage,
      openModal,
      closeModal,
      selectNext,
      selectPrev,
      handleImageKeydown
    }
  },
  components: {
    AnimatedMacaroni
  }
}
</script>

<template>
  <div id="galleryView" class="view">
    <AnimatedMacaroni />
    <div v-if="showModal" class="modalPopUp" tabindex="-1" aria-modal="true" role="dialog">
      <div class="modalImage">
        <img :src="images[selectedImage].src" alt="Wybrane zdjęcie z galerii Anetki" />
        <button @click="closeModal" class="close" aria-label="Zamknij podgląd zdjęcia">X</button>
      </div>
      <button
        @click="selectPrev"
        class="previous"
        :disabled="selectedImage === 0"
        aria-label="Poprzednie zdjęcie"
      >
        &lt;
      </button>
      <button
        @click="selectNext"
        class="next"
        :disabled="selectedImage === images.length - 1"
        aria-label="Następne zdjęcie"
      >
        &gt;
      </button>
    </div>
    <div class="galleryContent flex">
      <div v-for="(image, index) in images" :key="index + 1" class="row">
        <img
          :src="image.src"
          :loading="lazy"
          @click="openModal(index)"
          @keydown="handleImageKeydown($event, index)"
          tabindex="0"
          role="button"
          :aria-label="'Otwórz zdjęcie ' + (index + 1) + ' z galerii Anetki'"
          loading="lazy"
          alt="Zdjęcie z galerii Anetki"
        />
      </div>
    </div>
  </div>
</template>

<style src="./GalleryView.scss"></style>
