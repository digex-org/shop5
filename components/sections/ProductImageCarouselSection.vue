<template>
  <section class="flex flex-col md:flex-row space-y-4 md:space-y-0 md:space-x-4">
    <!-- Thumbnail Navigation -->
    <div class="relative flex md:flex-col space-x-2 md:space-y-2 md:space-x-0 overflow-auto">
      <swiper
          direction="vertical"
          slides-per-view="5"
          free-mode
          space-between="10"
          class="thumbnail-slider max-h-[400px] !hidden md:!block"
          @swiper="onThumbnailSwiper"
      >
        <swiper-slide
            v-for="(image, index) in images"
            :key="index"
            @click="onThumbnailClick(index)"
            class="cursor-pointer"
        >
          <img
              :src="image"
              :alt="'Thumbnail ' + index"
              class="rounded-lg object-cover w-16 h-16 border border-gray-200 hover:border-black"
              :class="index === activeIndex ? '!border-black border' : ''"
              loading="lazy"
          />
        </swiper-slide>
      </swiper>
    </div>

    <!-- Main Image Slider -->
    <div class="flex-grow w-full md:w-4/5 relative">
      <swiper
          ref="mainSwiper"
          @swiper="onMainSwiper"
          @slideChange="onSlideChange"
          loop
          class="main-swiper"
      >
        <swiper-slide v-for="(image, index) in images" :key="index">
          <div
              class="relative overflow-hidden w-full h-[500px] border-2 border-gray-300"
              @click="onImageClick"
              @mousemove="onImageHover"
              @mouseleave="onImageLeave"
          >
            <img
                ref="zoomedImage"
                :src="image"
                alt="Zoomable"
                class="absolute transition-transform duration-300 ease-in-out"
                :style="imageStyles"
                loading="lazy"
            />
          </div>
        </swiper-slide>
      </swiper>
    </div>
  </section>
</template>

<script setup>
import { ref, nextTick } from "vue";
import { Swiper, SwiperSlide } from "swiper/vue";
import "swiper/css";
import "swiper/css/free-mode";
import "swiper/css/navigation";

const props = defineProps({
  images: {
    type: Array,
    required: true,
  },
});

const activeIndex = ref(0);
const mainSwiperInstance = ref(null);
const thumbnailSwiperInstance = ref(null);

const isZoomed = ref(false); // Track if the image is zoomed
const imageStyles = ref({}); // Dynamic styles for zoom functionality

const zoomLevel = 2; // Zoom factor

const onMainSwiper = (swiper) => {
  mainSwiperInstance.value = swiper;
};

const onThumbnailSwiper = async (swiper) => {
  thumbnailSwiperInstance.value = swiper;
  await nextTick();
};

const onSlideChange = () => {
  if (mainSwiperInstance.value) {
    activeIndex.value =
        mainSwiperInstance.value.activeIndex % props.images.length;
  }
};

const onThumbnailClick = (index) => {
  if (mainSwiperInstance.value) {
    activeIndex.value = index;
    mainSwiperInstance.value.slideTo(index);
  }
};

const onImageClick = (event) => {
  if (isZoomed.value) {
    resetZoom();
    return;
  }

  const imageContainer = event.currentTarget;
  const rect = imageContainer.getBoundingClientRect();
  const clickX = event.clientX - rect.left;
  const clickY = event.clientY - rect.top;

  imageStyles.value = {
    transform: `scale(${zoomLevel})`,
    transformOrigin: `${clickX}px ${clickY}px`, // Align the zoom based on click position
  };

  isZoomed.value = true;
};

const onImageHover = (event) => {
  if (!isZoomed.value) return;

  const imageContainer = event.currentTarget;
  const rect = imageContainer.getBoundingClientRect();
  const hoverX = event.clientX - rect.left;
  const hoverY = event.clientY - rect.top;


  imageStyles.value = {
    transform: `scale(${zoomLevel})`,
    transformOrigin: `${hoverX}px ${hoverY}px`, // Follow the cursor position
  };
};
const resetZoom = () => {
  isZoomed.value = false;
  imageStyles.value = {
    transform: "scale(1)",
    left: "0px",
    top: "0px",
  };
};
</script>

<style scoped>
.thumbnail-slider {
  width: 64px;
  overflow: hidden;
  height: 100%;
  position: relative;
}
/* Ensure image and container styles are consistent */
.main-swiper img {
  max-height: 500px;
  width: 100%;
  object-fit: cover;
  position: absolute;
  transform-origin: top left; /* Ensure proper zoom alignment */
}

.relative.overflow-hidden {
  overflow: hidden; /* Clip the zoomed image */
}

</style>

