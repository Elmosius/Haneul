<script setup lang="ts">
import gsap from "gsap";
import Lenis from "lenis";
import { onMounted, ref } from "vue";
import ScrambleTextPlugin from "gsap/ScrambleTextPlugin";
import ScrollTrigger from "gsap/ScrollTrigger";

import Hero from "./components/UI/Hero";
import Intro from "./components/UI/Intro";

const isIntroCompleted = ref(true);

function handleIntroCompleted() {
  isIntroCompleted.value = true;
}

onMounted(() => {
  gsap.registerPlugin(ScrambleTextPlugin, ScrollTrigger);
  const lenis = new Lenis();
  lenis.on("scroll", ScrollTrigger.update);
  gsap.ticker.add((time: number) => lenis.raf(time * 1000));
  gsap.ticker.lagSmoothing(0);
});
</script>

<template>
  <!-- <Intro @intro-completed="handleIntroCompleted" /> -->
  <Hero v-if="isIntroCompleted" />
</template>

<style scoped></style>
