<template>
  <div>
    <SliderSection :slides="slides" />
    <VideoSection :video="'/videos/video.mp4'" />
    <div class="container m-auto my-12">
      <HeadingSection :header="'Vanilla Skin'" :class-name="'font-normal text-2xl'"/>
      <ProductGridSection :paginate="false" :products="products" class-name="grid gap-4 grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4" />
    </div>
    <CategoriesSection :limit="5"/>
<!--    <ShopSection />-->
    <VideoSection :video="'/videos/video.mp4'" />
<!--    <BannerSection :banner="'/images/banners/gothBanner.png'" />-->

  </div>
</template>

<script setup>
import '@fortawesome/fontawesome-svg-core/styles.css';
import {useProduct} from "~/composables/useProduct.js";
const { products } = useProduct();

const slides = [
  {
    backgroundImage: "/images/backgrounds/mainBanner.png",
  },
];
</script>

