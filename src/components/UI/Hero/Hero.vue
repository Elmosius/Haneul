<script setup lang="ts">
import gsap from "gsap";
import { onMounted, ref } from "vue";
import { SplitText } from "gsap/SplitText";
import ScrollTrigger from "gsap/ScrollTrigger";

gsap.registerPlugin(ScrollTrigger);

const hero = ref<HTMLDivElement | null>(null);
const title = ref<HTMLHeadingElement | null>(null);

const test = ref<HTMLDivElement | null>(null);
const img = ref<HTMLImageElement | null>(null);
const card = ref<HTMLDivElement | null>(null);
const taglineImg = ref<HTMLHeadingElement | null>(null);

const test2 = ref<HTMLDivElement | null>(null);
const card2 = ref<HTMLDivElement | null>(null);
const taglineImg2 = ref<HTMLHeadingElement | null>(null);

onMounted(() => {
  const tl = gsap.timeline({
    yoyo: true,
    repeat: -1,
    repeatDelay: 1,
  });
  const t = SplitText.create(title.value, { type: "chars" });
  const subline = title.value?.nextElementSibling;
  const t2 = SplitText.create(subline || "", { type: "chars" });

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

  gsap
    .timeline({
      scrollTrigger: {
        trigger: hero.value,
        start: "top top",
        end: "bottom center",
        scrub: 1,
        pin: true,
      },
    })
    .fromTo(
      test.value,
      {
        yPercent: -12,
        clipPath: "polygon(0 20px,100% 0,100% 100%,0 100%)",
      },
      {
        clipPath: "polygon(0 200px,100% 0,100% 100%,0 100%)",
        yPercent: -100,
        ease: "power4.inOut",
      },
    );

  gsap.set(card.value, {
    scale: 0.7,
  });

  const t1 = SplitText.create(taglineImg.value, { type: "chars" });

  gsap.set(t1.chars, {
    y: -50,
    clipPath: "inset(0 0 0 0)",
  });

  gsap
    .timeline({
      scrollTrigger: {
        trigger: test.value,
        start: "top bottom",
        end: "top center",
        scrub: 1,
        // pin: true,
        markers: true,
      },
    })
    .to(card.value, {
      scale: 1,
      ease: "power4.inOut",
    })
    .from(
      t1.chars,
      {
        stagger: 0.05,
        y: 0,
        clipPath: "inset(100% 0 0 0)",
        ease: "power4.inOut",
      },
      "-=0.5",
    )
    .to(card.value, {
      yPercent: -5,
      scale: 0.9,
      ease: "power4.inOut",
    });

  /// Image 2
  const t3 = SplitText.create(taglineImg2.value, { type: "chars" });

  gsap.set(test2.value, {
    yPercent: -100,
  });

  gsap.set(card2.value, {
    scale: 0.7,
  });

  gsap.set(t3.chars, {
    y: -50,
    clipPath: "inset(0 0 0 0)",
  });

  gsap
    .timeline({
      scrollTrigger: {
        trigger: test2.value,
        start: "top center",
        end: "bottom center",
        scrub: 1,
        markers: true,
      },
    })
    .to(card2.value, {
      scale: 1,
      ease: "power4.inOut",
    })
    .from(t3.chars, {
      stagger: 0.05,
      y: 0,
      clipPath: "inset(100% 0 0 0)",
      duration: 1,
    })
    .to(card2.value, {
      yPercent: -5,
      scale: 0.9,
      ease: "power4.inOut",
    });
});
</script>

<template>
  <section ref="hero">
    <div
      class="text-gwhite flex flex-col gap-5 justify-center items-center h-screen w-screen bg-night"
    >
      <h2 class="text-8xl" ref="title">Haneul (하늘)</h2>
      <p class="text-5xl font-medium">하늘에 담긴 너의 이야기 조각</p>
    </div>
  </section>

  <div class="bg-gwhite relative" ref="test">
    <div class="flex flex-col justify-center items-center">
      <div ref="card">
        <img
          src="/images/1.jpg"
          alt="1"
          class="w-full h-full object-cover rounded-xl grayscale-50"
          ref="img"
        />

        <div
          class="absolute inset-0 flex justify-center items-center top-[30%]"
        >
          <h2 class="text-gwhite font-semibold text-9xl" ref="taglineImg">
            서울빛
          </h2>
        </div>
      </div>
    </div>
  </div>

  <div class="relative bg-gwhite" ref="test2">
    <div class="flex justify-center items-center">
      <div ref="card2">
        <img
          src="/images/2.jpg"
          alt="2"
          class="w-full h-full object-cover rounded-xl grayscale-50"
        />
      </div>

      <div class="absolute inset-0 flex justify-center items-center">
        <h2 class="text-gwhite font-semibold text-9xl" ref="taglineImg2">
          하늘빛
        </h2>
      </div>
    </div>
  </div>
</template>

<style scoped></style>
