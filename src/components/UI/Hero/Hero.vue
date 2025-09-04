<script setup lang="ts">
import gsap from "gsap";
import { onMounted, ref } from "vue";
import { SplitText } from "gsap/SplitText";
import ScrollTrigger from "gsap/ScrollTrigger";

gsap.registerPlugin(ScrollTrigger);

const hero = ref<HTMLDivElement | null>(null);
const title = ref<HTMLHeadingElement | null>(null);

const test = ref<HTMLElement | null>(null);
const test2 = ref<HTMLElement | null>(null);
const test3 = ref<HTMLElement | null>(null);

const img = ref<HTMLElement | null>(null);
const img2 = ref<HTMLElement | null>(null);
const img3 = ref<HTMLElement | null>(null);

onMounted(() => {
  const t = SplitText.create(title.value, { type: "chars" });

  gsap.set(t.chars, {
    clipPath: "inset(0 0 0 0)",
  });
  gsap.from(t.chars, {
    clipPath: "inset(100% 0 0 0)",
    duration: 1,
    stagger: 0.05,
    ease: "power4.inOut",
  });

  gsap.to(title.value, {
    delay: 1,
    y: -80,
    opacity: 0,
    ease: "power4.inOut",
    duration: 1.5,
    onComplete: () => title.value?.classList.add("hidden"),
  });

  const tl = gsap.timeline({
    yoyo: true,
    repeat: -1,
    repeatDelay: 1,
  });

  const subline = title.value?.nextElementSibling;
  const t2 = SplitText.create(subline || "", { type: "chars" });

  tl.from(t2.chars, {
    delay: 2.5,
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

  gsap.set(test.value, {
    clipPath: "polygon(0 0, 100% 0, 100% 100%, 0% 100%)",
  });

  const text = test2.value?.children[1];
  const textSplit = SplitText.create(text || "", { type: "chars" });

  gsap
    .timeline({
      scrollTrigger: {
        trigger: hero.value,
        start: "top top",
        end: "bottom+=80% top",
        scrub: 0.2,
      },
    })
    .to(test.value, {
      yPercent: -100,
      clipPath: "polygon(0 25%, 100% 0, 100% 100%, 0% 100%)",
    })
    .to(img.value, {
      width: "100%",
      scale: 1,
    })
    .to(img.value, {
      scale: 0.65,
      width: "50%",
    })
    .from(
      textSplit.chars,
      {
        clipPath: "inset(100% 0 0 0)",
        stagger: 0.05,
      },
      "-=1",
    );

  const text2 = img2.value?.nextSibling;
  const textSplit2 = SplitText.create(text2, { type: "chars" });

  gsap
    .timeline({
      scrollTrigger: {
        trigger: test2.value,
        start: "bottom center",
        end: "bottom+=50% top",
        scrub: 0.2,
      },
    })
    .to(img2.value, {
      width: "100%",
      scale: 1,
    })
    .to(img2.value, {
      scale: 0.65,
      width: "50%",
    })
    .from(
      textSplit2.chars,
      {
        clipPath: "inset(100% 0 0 0)",
        stagger: 0.05,
      },
      "-=1",
    );

  const text3 = img3.value?.nextSibling;
  const textSplit3 = SplitText.create(text3, { type: "chars" });

  gsap
    .timeline({
      scrollTrigger: {
        trigger: test3.value,
        start: "top center",
        end: "bottom center",
        scrub: 0.2,
        // markers: true,
      },
    })
    .to(img3.value, {
      width: "100%",
      scale: 1,
    })
    .from(
      textSplit3.chars,
      {
        clipPath: "inset(100% 0 0 0)",
        stagger: 0.05,
      },
      "<",
    );
});
</script>

<template>
  <section ref="hero" class="relative">
    <div
      class="text-gwhite flex flex-col gap-5 justify-center items-center h-screen w-screen bg-night"
    >
      <h2 class="text-8xl" ref="title">Haneul (하늘)</h2>
      <p class="text-5xl font-medium">하늘에 담긴 너의 이야기 조각</p>
    </div>
  </section>

  <div class="relative w-screen">
    <div class="h-[150vh] w-screen"></div>

    <div
      class="absolute inset-0 bg-gwhite h-[150vh] w-screen z-30 overflow-hidden flex items-center justify-center"
      ref="test2"
    >
      <div class="scale-65 w-1/2" ref="img">
        <img
          src="/images/1.jpg"
          alt="1"
          class="w-full h-full object-cover origin-[bottom_center] brightness-75"
        />
      </div>

      <p class="absolute font-semibold text-8xl uppercase text-gwhite">Cloud</p>
    </div>

    <div class="absolute inset-0 bg-gwhite h-screen w-screen z-20" ref="test" />
  </div>

  <div
    class="relative bg-gwhite h-[150vh] w-screen flex items-center justify-center"
  >
    <div class="scale-65 w-1/2" ref="img2">
      <img
        src="/images/2.jpg"
        alt="2"
        class="w-full h-full object-cover origin-[bottom_center] brightness-75"
      />
    </div>

    <p class="absolute font-semibold text-8xl uppercase text-gwhite">light</p>
  </div>

  <div
    class="relative bg-gwhite h-[150vh] flex items-center justify-center"
    ref="test3"
  >
    <div class="scale-65 w-1/2" ref="img3">
      <img
        src="/images/3.jpg"
        alt="3"
        class="w-full h-full object-cover brightness-75"
      />
    </div>

    <p class="absolute font-semibold text-8xl uppercase text-gwhite">memory</p>
  </div>
</template>

<style scoped></style>
