<script setup lang="ts">
import Hero from "./components/UI/Hero";
import ScrambleTextPlugin from "gsap/ScrambleTextPlugin";
import { ScrollTrigger } from "gsap/ScrollTrigger";
import gsap from "gsap";
import Intro from "./components/UI/Intro";
import { ref } from "vue";

gsap.registerPlugin(ScrambleTextPlugin, ScrollTrigger);

const isIntroCompleted = ref(false);

function handleIntroCompleted() {
  isIntroCompleted.value = true;
}
</script>

<template>
  <main class="h-screen w-screen bg-night">
    <Intro @intro-completed="handleIntroCompleted" />
    <Hero v-if="isIntroCompleted" />
  </main>
</template>

<style scoped></style>
