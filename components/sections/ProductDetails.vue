<template>
  <div class="grid grid-cols-1 lg:grid-cols-2 gap-8 p-4">
<!--    <ProductImageCarouselSection :images="product.images" class="w-full" />-->
    <ProductImageCarouselSection :images="product.images"/>

    <div class="p-4 space-y-3">
      <!-- Product Title and Price -->
      <div>
        <p class="text-sm text-gray-400 font-semibold">Sustainable materials</p>
        <h1 class="text-2xl font-bold">{{ product.title }}</h1>
        <h1 class="text-xl mt-5 mb-10">{{ product.shortDescription }}</h1>
        <p class="text-base text-gray-600">{{ product.subtitle }}</p>
        <div class="my-2" v-if="product.sizes">
          <h3 class="text-xl mb-2 text-gray-900">Size</h3>
          <div class="flex gap-2">
            <button
                v-for="(size, index) in product.sizes"
                :key="index"
                class="border border-gray-400 py-2 px-4 rounded-md text-sm font-bold hover:bg-gray-200"
            >
              {{ size }}
            </button>
          </div>
        </div>

        <p class="text-xl my-2"><span
            v-if="product.originalPrice"
            class="text-gray-400 ml-2 line-through"
        >
              ${{ product.originalPrice }}
            </span>
          ${{ product.price }}</p>
        <AddToCartButton :product="product" :quantity="quantity" class="w-full !bg-black py-2 text-sm font-semibold hover:bg-gray-800" />

      </div>
      <!-- Product Description -->
      <div>
        <div class="overflow-hidden">
          <!-- Accordion Header -->
          <button
              @click="toggleAccordion"
              class="flex justify-between items-center w-full py-3 bg-transparent"
          >
            <span class="text-lg font-bold">Description</span>
            <font-awesome-icon
                :icon="isOpen ? ['fas', 'minus'] : ['fas', 'plus']"
                class="text-gray-600"
            />
          </button>
          <!-- Accordion Content -->
          <div
              v-show="isOpen"
              class="px-4 py-3 bg-white text-sm text-gray-700 leading-relaxed"
          >
            <p>{{ product.description }}</p>
            <ul class="list-disc pl-5 mt-2">
              <li>100% Cotton fabric</li>
              <li>Short sleeve crew neck T-shirt</li>
              <li>Screenprinted graphic on the front and back</li>
            </ul>
          </div>
          <hr />
        </div>
      </div>
      <div class="flex flex-col items-start mt-1">
        <span class="text-lg font-bold">Review</span>
        <div class="flex items-center mt-1">
          <div class="flex relative">
              <span
                  v-for="n in 5"
                  :key="n"
                  class="relative inline-block text-lg"
              >
                <font-awesome-icon
                    :icon="['fas', 'star']"
                    class="text-gray-400 text-sm"
                />
                <span
                    v-if="n <= Math.ceil(product.rating)"
                    class="absolute inset-0 overflow-hidden text-yellow-400"
                    :style="{ width: getStarFill(n) }"
                >
                  <font-awesome-icon :icon="['fas', 'star']" class="text-sm"/>
                </span>
              </span>
          </div>
          <span class="ml-2 text-sm text-gray-500">
              ({{ product.rating.toFixed(2) }})
            </span>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import {upperFirst} from "scule";
import {FontAwesomeIcon} from "@fortawesome/vue-fontawesome";

const props = defineProps({
  product: {
    type: Object,
    required: true
  }
});
const quantity = ref(1);
const isOpen = ref(false);

const toggleAccordion = () => {
  isOpen.value = !isOpen.value;
};

const updateQuantity = (newQuantity) => {
  quantity.value = newQuantity;
};

const getStarFill = (starIndex) => {
  if (props.product.rating >= starIndex) {
    return "100%"; // Full star
  }
  if (props.product.rating < starIndex - 1) {
    return "0%"; // Empty star
  }
  // Partially filled star
  return `${(props.product.rating - (starIndex - 1)) * 100}%`;
};
</script>
