<template>
  <div class="slider-container">
    <div class="slider">
      <div 
        v-for="(slide, index) in slidesComputed" 
        :key="index" 
        class="slide"
        :class="{ active: index === currentIndex }"
        :style="{ backgroundImage: slide.media.type === 'image' ? `url(${slide.media.src})` : 'none' }">
        <video v-if="slide.media.type === 'video'" :src="slide.media.src" autoplay loop muted></video>
        <h2 class="slide-title">{{ slide.title }}</h2>
      </div>
    </div>
    <button class="prev" @click="prevSlide">&#10094;</button>
    <button class="next" @click="nextSlide">&#10095;</button>
  </div>
</template>

<script setup>
import { ref, computed, onMounted, onBeforeUnmount, watch, nextTick } from 'vue';
import gsap from 'gsap';

const props = defineProps({
  loop: {
    type: Boolean,
    default: true,
  },
  slides: {
    type: Array,
    required: true,
  },
  autoplay: {
    type: Boolean,
    default: false,
  },
  autoplayDelay: {
    type: Number,
    default: 3000,
  },
});

const currentIndex = ref(0);
let autoplayInterval = null;

const slidesComputed = computed(() => {
  if (props.slides.length < 3) {
    return [...props.slides, ...props.slides, ...props.slides].slice(0, 3);
  }
  return props.slides;
});

const nextSlide = () => {
  if (props.loop || currentIndex.value < slidesComputed.value.length - 1) {
    currentIndex.value = (currentIndex.value + 1) % slidesComputed.value.length;
    animateSlide();
  }
};

const prevSlide = () => {
  if (props.loop || currentIndex.value > 0) {
    currentIndex.value = (currentIndex.value - 1 + slidesComputed.value.length) % slidesComputed.value.length;
    animateSlide();
  }
};

const animateSlide = () => {
  nextTick(() => {
    gsap.fromTo(
      '.slide-title',
      { clipPath: 'inset(100% 0 0 0)', opacity: 0 },
      { clipPath: 'inset(0 0 0 0)', opacity: 1, duration: 1, ease: 'power2.out' }
    );
  });
};

const startAutoplay = () => {
  if (props.autoplay) {
    autoplayInterval = setInterval(nextSlide, props.autoplayDelay);
  }
};

const stopAutoplay = () => {
  if (autoplayInterval) {
    clearInterval(autoplayInterval);
  }
};

watch(currentIndex, () => {
  animateSlide();
});

onMounted(() => {
  startAutoplay();
  animateSlide();
});

onBeforeUnmount(() => {
  stopAutoplay();
});
</script>

<style scoped>
.slider-container {
  position: relative;
  width: 100%;
  max-width: 800px;
  height: 400px;
  overflow: hidden;
}

.slider {
  display: flex;
  transition: transform 0.5s ease-in-out;
}

.slide {
  min-width: 100%;
  height: 100%;
  background-size: cover;
  background-position: center;
  position: relative;
}

.slide-title {
  position: absolute;
  bottom: 20px;
  left: 20px;
  font-size: 24px;
  color: white;
  clip-path: inset(100% 0 0 0);
  opacity: 0;
}

.active .slide-title {
  animation: slideIn 1s forwards;
}

@keyframes slideIn {
  from {
    clip-path: inset(100% 0 0 0);
    opacity: 0;
  }
  to {
    clip-path: inset(0 0 0 0);
    opacity: 1;
  }
}

.prev, .next {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  background: rgba(0, 0, 0, 0.5);
  color: white;
  border: none;
  padding: 10px;
  cursor: pointer;
}

.prev {
  left: 10px;
}

.next {
  right: 10px;
}
</style>
