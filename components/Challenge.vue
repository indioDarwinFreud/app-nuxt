<template>
    <div class="slider-container">
      <div class="slider">
        <div 
          v-for="(slide, index) in slidesComputed" 
          :key="index"
          class="slide"
          v-show="index === currentIndex"
        >
          <!-- Mostrar imagen -->
          <img v-if="slide.media.type === 'image'" :src="slide.media.src" class="slide-image" />
  
          <!-- Mostrar video -->
          <video 
            v-else-if="slide.media.type === 'video'" 
            :src="slide.media.src" 
            :key="slide.media.src" 
            class="slide-video" 
            autoplay 
            loop 
            muted 
            playsinline
          ></video>
  
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
  
  // Asegurar que haya al menos 3 elementos en slides
  const slidesComputed = computed(() => {
    return props.slides.length >= 3 ? props.slides : [...props.slides, ...props.slides, ...props.slides].slice(0, 3);
  });
  
  const nextSlide = () => {
    currentIndex.value = (currentIndex.value + 1) % slidesComputed.value.length;
    animateSlide();
  };
  
  const prevSlide = () => {
    currentIndex.value = (currentIndex.value - 1 + slidesComputed.value.length) % slidesComputed.value.length;
    animateSlide();
  };
  
  const animateSlide = () => {
    nextTick(() => {
      gsap.fromTo(
        ".slide",
        { opacity: 0 },
        { opacity: 1, duration: 1, ease: "power2.out" }
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
  
  // Verificar si los slides se cargan correctamente
  watch(slidesComputed, (newSlides) => {
    console.log("Slides actualizados:", newSlides);
  }, { immediate: true });
  
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
    max-width: 1000px;
    height: 500px;
    overflow: hidden;
  }
  
  .slider {
    display: flex;
    transition: transform 0.5s ease-in-out;
  }
  
  .slide {
    min-width: 100%;
    height: 100%;
    position: relative;
    display: flex;
    align-items: center;
    justify-content: center;
  }
  
  .slide-image {
    width: 100%;
    height: 100%;
    object-fit: cover;
  }
  
  .slide-video {
    width: 100%;
    height: 100%;
    object-fit: cover;
  }
  
  .slide-title {
    position: absolute;
    bottom: 20px;
    left: 20px;
    font-size: 24px;
    color: white;
    background: rgba(0, 0, 0, 0.5);
    padding: 5px 10px;
    border-radius: 5px;
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
  