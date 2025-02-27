<template>
  <div class="banner">
    <div class="slides">
      <div v-for="(image, index) in images" :key="index" class="slide" :class="{ active: index === currentSlide }">
        <img :src="image.src" :alt="image.alt" />
      </div>
    </div>
    <div class="dots">
      <span v-for="(image, index) in images" :key="index" class="dot" :class="{ active: index === currentSlide }" @click="goToSlide(index)"></span>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted, onUnmounted, defineProps } from 'vue';

const props = defineProps<{
  images: { src: string; alt: string }[];
}>();

const currentSlide = ref(0);
let intervalId: number | undefined;

const goToSlide = (index: number) => {
  currentSlide.value = index;
};

const nextSlide = () => {
  currentSlide.value = (currentSlide.value + 1) % props.images.length;
};

onMounted(() => {
  intervalId = setInterval(nextSlide, 3000);
});

onUnmounted(() => {
  if (intervalId) {
    clearInterval(intervalId);
  }
});
</script>

<style scoped>
.banner {
  position: relative;
  width: 100%;
  overflow: hidden;
  height: 200px; /* Initial height, adjust with media queries */
  border-radius: 0.5rem;
}

.slides {
  display: flex;
  transition: transform 0.5s ease-in-out;
}

.slide {
  min-width: 100%;
  height: 100%;
    display: flex; /* Use flexbox to center the image */
  justify-content: center; /* Center horizontally */
  align-items: center;
}

.slide img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  object-position: center;
}

.dots {
  position: absolute;
  bottom: 10px;
  left: 50%;
  transform: translateX(-50%);
  display: flex;
  gap: 0.5rem;
}

.dot {
  width: 0.5rem;
  height: 0.5rem;
  border-radius: 50%;
  background-color: rgba(255, 255, 255, 0.5);
  cursor: pointer;
}

.dot.active {
  background-color: white;
}

/* Responsive adjustments */
@media (min-width: 640px) {
  .banner {
    height: 400px; /* Larger height for larger screens */
  }
}
</style>
