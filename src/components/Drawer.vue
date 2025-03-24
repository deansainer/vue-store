<script setup>
import { inject, ref, computed } from 'vue';
import CartItemList from './CartItemList.vue';
import InfoBlock from './InfoBlock.vue';
import axios from 'axios';

const { closeDrawer, cartItems } = inject('cart');

const orderId = ref(null);

// Реактивний cartTotal
const cartTotalComputed = computed(() =>
  cartItems.value.reduce((sum, item) => sum + item.price, 0)
);

// creating order
async function createOrder() {
  try {
    const { data } = await axios.post('https://f5f2b5caba228578.mokky.dev/orders', {
      cartTotal: cartTotalComputed.value,
      items: cartItems.value,
    });
    cartItems.value.forEach((item) => (item.isAdded = false));
    cartItems.value = []; // Clear the cart

    orderId.value = data.id;
  } catch (error) {
    console.log(error);
  }
}
</script>

<template>
  <div>
    <div @click="closeDrawer" class="bg-black fixed top-0 left-0 h-full w-full opacity-70 z-10"></div>
    <div class="bg-white flex h-full fixed top-0 right-0 w-96 h-full z-20 p-5 flex-col">
      
      <!-- Відображення стану кошика -->
      <div v-if="cartTotalComputed <= 0">
        <InfoBlock 
          v-if="!orderId" 
          title="Empty Cart" 
          description="Just add some items you liked" 
          imageURL="/package-icon.png"
        />
        <InfoBlock 
          v-else 
          title="Your order is placed!" 
          description="Your confirmation is coming to your email address" 
          imageURL="/order-success-icon.png"
        />
      </div>

      <div class="h-full flex flex-col" v-if="cartTotalComputed > 0">
        <div class="flex items-center gap-5 h-10">
          <img @click="closeDrawer" class="w-8 h-8 cursor-pointer mt-1 opacity-40 hover:opacity-90 transition" src="/arrow-back.svg">
          <span class="text-2xl">Cart</span>
        </div>

        <CartItemList/>

        <div class="mt-auto">
          <div class="flex justify-between">
            <span>Total: </span>
            <span>${{ cartTotalComputed }}</span>
          </div>

          <div class="flex justify-between">
            <span>Fee 5%: </span>
            <span>${{ (cartTotalComputed / 20).toFixed(2) }}</span>
          </div>

          <button :disabled="cartTotalComputed <= 0" @click="createOrder" class="text-white mt-4 bg-sky-800 hover:bg-sky-900 active:bg-sky-950 disabled:bg-slate-300 disabled:cursor-default transition py-3 cursor-pointer w-full rounded-xl">
            Order
          </button>
        </div>
      </div>
    </div>
  </div>
</template>
