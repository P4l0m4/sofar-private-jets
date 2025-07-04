<script setup lang="ts">
import { isDesktop, isMobile } from "@/utils/functions";
import { colors } from "@/utils/colors";
import { ref } from "vue";
import preview from "@/assets/images/our-story/our-fleet.webp";

const desktopScreen = ref(isDesktop());
const mobileScreen = ref(isMobile());

window.addEventListener("resize", () => {
  desktopScreen.value = isDesktop();
  mobileScreen.value = isMobile();
});

const questions = [
  {
    title: "Can I bring luggage on board?",
    answer:
      "Yes, you can bring luggage on board. Each aircraft in the Sofar fleet has a specific luggage capacity, and we’ll advise you on what fits best depending on your destination and number of passengers. Let us know your needs in advance to ensure smooth boarding.",
  },
  {
    title: "Are pets allowed on Sofar flights?",
    answer:
      "Absolutely. Pets are welcome on all Sofar flights. We kindly ask you to provide your pet’s details during booking so we can prepare the aircraft accordingly and ensure a comfortable experience for both of you.",
  },
  {
    title: "What amenities are available onboard?",
    answer:
      "Sofar flights offer premium comfort and essentials: inflight refreshments, phone charging outlets, and, upon request, tailored amenities such as snacks, drinks, and pet-friendly accessories. While our flights are short, your experience remains refined and seamless.",
  },
  {
    title: "How far can I travel with Sofar’s fleet?",
    answer:
      "Sofar specializes in regional flights of up to 2 hours. With bases in New York, Miami, and Los Angeles, our fleet covers a wide range of popular destinations such as The Hamptons, Palm Beach, Las Vegas, and the Caribbean. Our team will help you find the best route based on your needs.",
  },
];

const label = ref("Get in touch");

function copyToClipboard() {
  navigator.clipboard.writeText("hq@flysofar.com");
  label.value = "Email copied to clipboard!";
  setTimeout(() => {
    label.value = "Get in touch";
  }, 1000);
}

useHead({
  title: "Our fleet | Premium private jets for every journey | Sofar",
  meta: [
    {
      name: "description",
      content:
        "Discover our premium private jets for every journey. At Sofar, we offer a curated fleet of private jets designed for luxury, efficiency, and comfort.",
    },
  ],
});

useJsonld(() => ({
  "@context": "https://schema.org",
  "@type": "WebSite",
  name: "Our fleet | Premium private jets for every journey | Sofar",
  url: window.location.href,
}));
</script>
<template>
  <picture class="services-banner">
    <source
      media="(min-width: 1100px)"
      srcset="
        @/assets/images/home/our-fleet-sofar-private-jet-charter-desktop.webp
      "
    />

    <div class="services-banner__headlines">
      <NuxtLink
        class="button-primary--dark rounded-button"
        :to="desktopScreen ? '/booking' : '/request-a-charter'"
        style="z-index: 1"
        >{{ !desktopScreen ? "Fly" : "Booking" }}</NuxtLink
      >
      <NuxtLink class="services-banner__headlines__logo" to="/">
        <img src="@/assets/images/logo-light.svg"
      /></NuxtLink>

      <EmergencyBubble v-if="desktopScreen" />
    </div>

    <QuoteFormDesktop
      parent="private-jet"
      v-if="desktopScreen"
      :color="colors['secondary-color']"
    />
    <img
      class="services-banner__img"
      src="@/assets/images/our-story/our-fleet-sofar-private-jet-mobile.webp"
      alt="private jet charter banner image"
    />
  </picture>

  <ServicesUnderBanner
    title="Our Fleet"
    subtitle="Premium Private Jets for Every Journey"
    text="At Sofar, we offer a curated fleet of private jets designed for luxury, efficiency, and comfort. Whether you’re traveling for business or leisure, our Phenom 100 and Phenom 300 jets are tailored for your needs, ensuring you reach your destination swiftly and in style."
    :image="preview"
    ><NuxtLink
      class="button-primary--light"
      to="mailto:hq@flysofar.com"
      @click="copyToClipboard()"
      :style="{ width: mobileScreen ? '100%' : 'fit-content' }"
      >{{ label }}</NuxtLink
    ></ServicesUnderBanner
  ><OurFleet :showGallery="true" />
  <AboutMakeItAnywhere />
  <AboutSofarPilots />
  <ServicesMiniFAQ :questions />
</template>
<style lang="scss" scoped>
@import "@/styles/planes.scss";
</style>
