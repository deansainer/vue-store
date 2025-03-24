<script setup>
import Header from './components/Header.vue'
import {ref, watch, computed, provide } from 'vue'
import Drawer from './components/Drawer.vue'

// states
const isDrawerOpen = ref(false)
const cartItems = ref([])


function openDrawer() {
  isDrawerOpen.value = true
}

function closeDrawer() {
  isDrawerOpen.value = false
}

provide('cart', {
  cartItems,
  openDrawer,
  closeDrawer,
  addToCart,
  removeFromCart,
})

// saving items into local storage
watch(cartItems, () => {
  localStorage.setItem('cartItems', JSON.stringify(cartItems.value))
},
  {deep: true}
)


function addToCart(item) {
  cartItems.value.push(item)
  item.isAdded = true
}

function removeFromCart(item) {
  cartItems.value = cartItems.value.filter((cartItem) => cartItem.id !== item.id)
  item.isAdded = false
}

// counting total
const cartTotal = computed(() =>
  cartItems.value.reduce((total, item) => total + item.price, 0).toFixed(2),
)

</script>

<template>
  <div class="text-2xl w-4/5 m-auto bg-white min-h-screen p-4 rounded-xl mt-20 shadow-xl">
    <Drawer :cartTotal="cartTotal" :createOrder="createOrder" v-if="isDrawerOpen" />
    <Header :cartTotal="cartTotal" />

    <div>
      <router-view>
        
      </router-view>
    </div>
  </div>
  
</template>
