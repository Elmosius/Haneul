<script setup lang="ts">
import gsap from "gsap";
import { VueLenis } from "lenis/vue";
import ScrambleTextPlugin from "gsap/ScrambleTextPlugin";
import { ScrollTrigger } from "gsap/ScrollTrigger";
import { ref, watchEffect } from "vue";
import Hero from "./components/UI/Hero";
import Intro from "./components/UI/Intro";
import Gallery from "./components/UI/Gallery";

gsap.registerPlugin(ScrambleTextPlugin, ScrollTrigger);

const isIntroCompleted = ref(false);

function handleIntroCompleted() {
  isIntroCompleted.value = true;
}

const lenisRef = ref();

watchEffect((onInvalidate) => {
  if (!lenisRef.value?.lenis) return;

  lenisRef.value.lenis.on("scroll", ScrollTrigger.update);

  function update(time) {
    lenisRef.value.lenis.raf(time * 1000);
  }
  gsap.ticker.add(update);

  gsap.ticker.lagSmoothing(0);

  onInvalidate(() => {
    gsap.ticker.remove(update);
  });
});
</script>

<template>
  <VueLenis root ref="lenisRef" :options="{ autoRaf: false }" />
  <main class="h-screen w-screen bg-night">
    <Intro @intro-completed="handleIntroCompleted" />
    <Hero v-if="isIntroCompleted" />
    <Gallery v-if="isIntroCompleted" />
  </main>
</template>

<style scoped></style>
