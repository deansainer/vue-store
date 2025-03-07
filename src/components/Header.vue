<script setup>
import { inject } from 'vue';
const {openDrawer, closeDrawer} = inject('cart')

defineProps({
  cartTotal: Number
})

</script>

<template>
    <header class="flex justify-between border-b p-4 border-gray-300">
      <div class="flex items-center">
        <img class="w-22" alt="logo" src="/carhartt-logo.svg">
        <div class="ml-4">
          <h1 class="text-xl font-bold uppercase">Carhartt</h1>
          <p class="text-slate-500 text-base">Work In Progress</p>
        </div>
      </div>


    <ul class="flex items-center">
      <li @click="openDrawer" class="ml-8 flex items-center text-slate-500 cursor-pointer hover:text-black">
        <img src="/cart.svg">
        <span class="text-sm ml-2">${{cartTotal}}</span>
      </li>

      <li class="ml-4 flex items-center text-slate-500 cursor-pointer hover:text-black">
        <img src="/heart.svg">
        <span class="text-sm ml-2">Favorites</span>
      </li>

      <li class="ml-4 flex items-center text-slate-500 cursor-pointer hover:text-black">
        <img src="/profile.svg">
        <span class="text-sm ml-2">Profile</span>
      </li>
    </ul>

    </header>
</template>