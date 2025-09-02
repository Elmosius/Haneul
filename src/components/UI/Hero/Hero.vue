<script setup lang="ts">
import gsap from "gsap";
import { onMounted, ref } from "vue";
import { SplitText } from "gsap/SplitText";

const hero = ref<HTMLDivElement | null>(null);
const title = ref<HTMLHeadingElement | null>(null);

onMounted(() => {
  const tl = gsap.timeline({
    yoyo: true,
    repeat: -1,
    repeatDelay: 1,
  });
  const t = SplitText.create(title.value, { type: "chars" });
  const subline = title.value?.nextElementSibling;
  const t2 = SplitText.create(subline || "", { type: "chars" });

  gsap.from(t.chars, {
    delay: 2.5,
    opacity: 0,
    duration: 1,
    stagger: 0.05,
    ease: "power4.inOut",
  });

  gsap.to(title.value, {
    delay: 3.5,
    autoAlpha: 0,
    y: -80,
    ease: "power4.inOut",
    duration: 2,
  });

  tl.from(t2.chars, {
    delay: 4,
    stagger: {
      each: 0.05,
      from: "end",
    },
    opacity: 0,
    duration: 1,
  })
    .to(subline, {
      duration: 3,
      ease: "power1.in",
      scrambleText: "A sky filled with fragments of your story.",
    })
    .to(subline, {
      delay: 2,
      duration: 2,
      ease: "power1.in",
      scrambleText: "하늘에 담긴 너의 이야기 조각",
    });
});
</script>

<template>
  <section ref="hero">
    <div
      class="text-gwhite flex flex-col gap-5 justify-center items-center h-screen w-screen bg-night"
    >
      <h2 class="text-8xl" ref="title">Haneul (하늘)</h2>
      <p class="text-5xl">하늘에 담긴 너의 이야기 조각</p>
    </div>
  </section>
</template>

<style scoped></style>
