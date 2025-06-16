<script setup>
import { ref, computed } from "vue";
import socksGreenImage from "@/assets/images/Luigi_socks.webp";
import socksRedImage from "@/assets/images/mario_socks.avif";
import ReviewList from "./ReviewList.vue";
import ReviewForm from "./ReviewForm.vue";

const emit = defineEmits(["add-to-card", "remove-from-cart"]);
const props = defineProps({
    premium: {
        type: Boolean,
        required: true
    },
    cart: {
        type: Array,
        required: false,
        default: () => []
    }
})
const selectedVariant = ref(0);
const product = ref("Socks");
const brand = ref("Vue Mastery");
const variants = ref([
    { id: 2234, color: "green", image: socksGreenImage, quantity: 5 },
    { id: 2235, color: "red", image: socksRedImage, quantity: 5 },
]);
const details = ref(["50% cotton", "30% wool", "20% polyester"]);

const addToCard = () => {
    const variant = variants.value[selectedVariant.value];
    if (variant.quantity > 0) {
        emit("add-to-card", variant.id);
        variant.quantity--;
    }
};
const removeFromCart = () => {
    const variant = variants.value[selectedVariant.value];
    emit("remove-from-cart", variant.id);
    if (variant.quantity < 10 && props.cart.filter(id => id === variant.id).length < 10) {
        variant.quantity++;
    }
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
        <div style="display: flex; gap: 10px; align-items: center;">
          <button
            class="button"
            :class="{ disabledButton: !inStock }"
            @click="addToCard"
            :disabled="!inStock"
          >
            Add to cart
          </button>
          <button
            class="button"
            :class="{ disabledButton: props.cart.filter(id => id === variants[selectedVariant].id).length === 0 }"
            @click="removeFromCart"
            :disabled="props.cart.filter(id => id === variants[selectedVariant].id).length === 0"
          >
            Remove from cart
          </button>
        </div>
      </div>
    </div>
    <ReviewList v-if="reviews.length > 0" :reviews="reviews"></ReviewList>
    <ReviewForm @review-submitted="addReview"></ReviewForm>
 </div>
</template>