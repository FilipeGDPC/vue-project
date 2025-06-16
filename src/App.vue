<script setup>
import { ref } from "vue";
import ProductDisplay from "@/components/ProductDisplay.vue";

const cart = ref([]);
const premium = ref(true);

const updateCart = (id) => {
  cart.value.push(id);
};

const removeFromCart = (id) => {
  const index = cart.value.lastIndexOf(id);
  if (index !== -1) {
    cart.value.splice(index, 1);
  }
};
</script>

<template>
  <div class="nav-bar"></div>
  <div class="cart">Cart({{ cart.length }})</div>
  <ProductDisplay :premium="premium" 
    @add-to-card="updateCart"
    @remove-from-cart="removeFromCart"
    :cart="cart"
  ></ProductDisplay>
  
</template>

<style scoped>

</style>
