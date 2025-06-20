<script setup lang="ts">
import { isDesktop } from "@/utils/functions";
import { colors } from "@/utils/colors";
import { ref } from "vue";
import family from "@/assets/images/private-jet-charter.webp";

const desktopScreen = ref(isDesktop());

window.addEventListener("resize", () => {
  desktopScreen.value = isDesktop();
});

const story = await useAsyncStoryblok("private-jet-charter", {
  version: "published",
});

const questions = [
  {
    title: "Is ground transportation included ?",
    answer:
      "Ground transportation is not automatically included with your flight. However, at SOFAR, we can arrange ground transportation upon request to ensure a seamless travel experience from door to door. Please let us know your needs when booking.",
  },
  {
    title: "Are children allowed on private jets ?",
    answer:
      "Yes, children are welcome on board SOFAR flights. For safety reasons, we may require appropriate seating accommodations depending on the age and size of the child. Our team will guide you to ensure a comfortable and safe journey for your family.",
  },
  {
    title: "Can I book multi-leg ?",
    answer:
      "Absolutely. SOFAR offers the flexibility to organize multi-leg itineraries to fit your travel plans. Whether you need to visit several destinations in one trip or arrange complex schedules, our team will coordinate every detail for you.",
  },
  {
    title: "Can I bring golf clubs, skis, or a surfboard ?",
    answer:
      "Yes, you can bring sports equipment like golf clubs, skis, or a surfboard on board. Please inform us in advance so we can select the most suitable aircraft and ensure proper space and handling for your equipment.",
  },
];

const videoRef = ref<HTMLVideoElement>();
const videoRef2 = ref<HTMLVideoElement>();
const videoRef3 = ref<HTMLVideoElement>();

function onError(e: Event) {
  console.error("Erreur de chargement de la vidéo :", e);
}

onMounted(() => {
  if (videoRef.value) {
    const p = videoRef.value.play();
    if (p instanceof Promise) {
      p.catch((err) => {
        console.warn("Autoplay bloqué par le navigateur :", err);
      });
    }
  }
  if (videoRef2.value) {
    const p = videoRef2.value.play();
    if (p instanceof Promise) {
      p.catch((err) => {
        console.warn("Autoplay bloqué par le navigateur :", err);
      });
    }
  }
  if (videoRef3.value) {
    const p = videoRef3.value.play();
    if (p instanceof Promise) {
      p.catch((err) => {
        console.warn("Autoplay bloqué par le navigateur :", err);
      });
    }
  }
});

useHead({
  title: "Private Jet Charters | Reliable & Flexible Air Travel | Sofar",
  meta: [
    {
      name: "description",
      content:
        "Experience the convenience and reliability of private jet charters with Sofar Aviation. Flexible scheduling and personalized services for all your travel needs.",
    },
  ],
});

useJsonld(() => ({
  "@context": "https://schema.org",
  "@type": "WebSite",
  name: "Private Jet Charters | Reliable & Flexible Air Travel | Sofar",
  url: window.location.href,
}));
</script>
<template>
  <picture class="index__banner">
    <source
      media="(min-width: 1100px)"
      :srcset="story.content.bannerImageDesktop.filename"
    />

    <div class="index__banner__headlines">
      <NuxtLink
        class="button-primary--dark rounded-button"
        :to="desktopScreen ? '/booking' : '/request-a-charter'"
        style="z-index: 1"
        >{{ !desktopScreen ? "Fly" : "Booking" }}</NuxtLink
      >

      <NuxtLink class="index__banner__headlines__logo" to="/">
        <img src="@/assets/images/logo-light.svg"
      /></NuxtLink>

      <EmergencyBubble v-if="desktopScreen" style="z-index: 1" />
    </div>

    <video
      ref="videoRef"
      class="auto-video"
      src="@/assets/videos/private jet charter.mp4"
      autoplay
      muted
      loop
      playsinline
      preload="auto"
      @error="onError"
      v-if="desktopScreen"
    />

    <video
      ref="videoRef"
      class="auto-video"
      src="@/assets/videos/private-jet-charter-mobile.mp4"
      autoplay
      muted
      loop
      playsinline
      preload="auto"
      @error="onError"
      v-if="!desktopScreen"
    />

    <QuoteFormDesktop
      parent="family-jet"
      v-if="desktopScreen"
      :color="colors['secondary-color']"
    />

    <img
      class="index__banner__img"
      :src="story.content.bannerImageMobile.filename"
      alt="banner image"
    />
  </picture>

  <ServicesUnderBanner
    title="Private Jet Charter"
    subtitle="Seamless, Exclusive, and Tailored for Your Needs"
    text="Luxury, efficiency, and flexibility—Sofar redefines private aviation by offering on-demand private jet charters tailored to your schedule. Whether for business, leisure, or last-minute travel, our fleet ensures a smooth and stress-free journey."
    :image="family"
  />

  <ServicesRequestProcess />

  <video
    class="full-width-video"
    src="@/assets/videos/family jet charter sofar.mp4"
    playsinline
    preload="auto"
    ref="videoRef2"
    autoplay
    muted
    loop
    v-if="desktopScreen"
  />
  <video
    class="full-width-video"
    src="@/assets/videos/family-jet-charter-mobile.mp4"
    playsinline
    preload="auto"
    ref="videoRef2"
    autoplay
    muted
    loop
    v-if="!desktopScreen"
  />
  <OurFleet />

  <ServicesPerksList />

  <ServicesOurServices />

  <video
    class="full-width-video"
    src="@/assets/videos/homepage-desktop.mp4"
    playsinline
    preload="auto"
    ref="videoRef2"
    autoplay
    muted
    loop
  />
  <ServicesMiniFAQ :questions />
</template>
<style lang="scss" scoped>
@import "@/styles/planes.scss";
</style>
