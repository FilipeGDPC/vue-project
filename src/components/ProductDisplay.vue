<script setup>
import { ref, computed } from "vue";
import socksGreenImage from "@/assets/images/Luigi_socks.webp";
import socksRedImage from "@/assets/images/mario_socks.avif";
import ReviewList from "./ReviewList.vue";
import ReviewForm from "./ReviewForm.vue";

const props = defineProps({
    premium: {
        type: Boolean,
        required: true
    }
})
const emit = defineEmits(["add-to-card"]);
const selectedVariant = ref(0);
const product = ref("Socks");
const brand = ref("Vue Mastery");
const variants = ref([
    { id: 2234, color: "green", image: socksGreenImage, quantity: 50 },
    { id: 2235, color: "red", image: socksRedImage, quantity: 0 },
]);
const details = ref(["50% cotton", "30% wool", "20% polyester"]);

const addToCard = () => {
    emit("add-to-card", variants.value[selectedVariant.value].id);
};
const updateVariant = (index) => {
  selectedVariant.value = index;
};

const addReview = (review) => {
  reviews.value.push(review);
}

const reviews = ref([])

const title = computed(() => {
  return brand.value + " " + product.value;
});
const image = computed(() => {
    return variants.value[selectedVariant.value].image
})
const inStock = computed(() => {
    return variants.value[selectedVariant.value].quantity > 0 
})
const shipping = computed(() => {
    if (props.premium) {
        return "Free"
    }
    return 2.99
})
</script>

<template>
    <div class="product-display">
    <div class="product-container">
      <div class="product-image">
        <img :src="image" alt="Socks Green" />
      </div>

      <div class="product-info">
        <div class="header">
          <h1>{{ title }}</h1>
        </div>
        <p v-if="inStock" >In Stock</p>
        <p v-else>Out of Stock</p>
        <p> shipping {{ shipping }}</p>

        <ul>
          <li v-for="detail in details">{{ detail }}</li>
        </ul>
        <div
          v-for="(variant, index) in variants"
          :key="variant.id"
          @click="updateVariant(index)"
          class="color-circle"
          :style="{ backgroundColor: variant.color }"
        ></div>
        <button
          class="button"
          :class="{ disabledButton: !inStock }"
          @click="addToCard"
          :disabled="!inStock"
        >
          Add to cart
        </button>
      </div>
    </div>
    <ReviewList :reviews="reviews"></ReviewList>
    <ReviewForm @review-submitted="addReview"></ReviewForm>
 </div>
</template>