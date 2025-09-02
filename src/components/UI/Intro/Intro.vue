<script setup lang="ts">
import gsap from "gsap";
import { onMounted, ref } from "vue";

const emit = defineEmits<{
  (e: "intro-completed"): void;
}>();

const intro = ref<HTMLDivElement | null>(null);
const square = ref<HTMLDivElement | null>(null);
const h1 = ref<HTMLHeadingElement | null>(null);

const displayNumber = ref(0);
const number = {
  value: 0,
};

onMounted(() => {
  const tl = gsap.timeline();

  tl.to(number, {
    value: 100,
    duration: 2,
    ease: "none",
    onUpdate: () => {
      displayNumber.value = Math.round(number.value);
    },
    onComplete: () => {
      gsap.to(h1.value, {
        opacity: 0,
        y: -50,
      });
    },
  }).to(
    square.value,
    {
      duration: 2.5,
      keyframes: [
        {
          duration: 1,
          ease: "power4.inOut",
          height: "80px",
          width: "80px",
        },
        {
          height: "20px",
          width: "20px",
          ease: "circ.inOut",
          rotate: 90,
        },
        {
          ease: "circ.inOut",
          height: "80px",
          width: "80px",
        },
        {
          height: "20px",
          width: "20px",
          ease: "circ.inOut",
          rotate: 125,
        },
        {
          ease: "power4.inOut",
          height: "80px",
          width: "80px",
        },
        {
          height: "20px",
          width: "20px",
          ease: "power4.inOut",
          rotate: 180,
        },
        {
          ease: "power4.inOut",
          height: "100%",
          width: "100%",
          onComplete: () => {
            intro.value?.classList.add("hidden");
            square.value?.parentElement?.classList.add("hidden");
            emit("intro-completed");
          },
        },
      ],
    },
    0,
  );
});
</script>

<template>
  <section ref="intro">
    <div class="bg-gwhite flex justify-end items-start w-screen h-screen p-15">
      <h1 class="text-night font-medium text-7xl" ref="h1">
        {{ displayNumber }}
      </h1>
    </div>
  </section>

  <div class="absolute inset-0 flex items-center justify-center h-screen">
    <div class="bg-night rotate-45" ref="square"></div>
  </div>
</template>

<style scoped></style>
