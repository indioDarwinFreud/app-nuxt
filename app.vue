<template>
  <div class="flex justify-center items-center min-h-screen bg-black">
    <Challenges :slides="slidesData" autoplay />
  </div>
</template>

<script setup>
import Challenges from "@/components/Challenge.vue";

const slidesData = [
  {
    title: "Imagen 1",
    media: {
      type: "image",
      src: "/multimedia/imagen1.jpg",
      w: "1080",
      h: "1920",
    },
  },
  {
    title: "Video 1",
    media: {
      type: "video",
      src: "/multimedia/video.mp4", 
      w: "1080",
      h: "1920",
    },
  },
  {
    title: "Imagen 2",
    media: {
      type: "image",
      src: "/multimedia/imagen2.jpg",
      w: "1080",
      h: "1920",
    },
  },
];
</script>
