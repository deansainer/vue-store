<script setup>
import CardList from './components/CardList.vue';
import Header from './components/Header.vue';
import Drawer from './components/Drawer.vue';
import axios from 'axios'


</script>

<template>
  <div class="text-2xl w-4/5 m-auto bg-white min-h-screen p-4 rounded-xl mt-20 shadow-xl">
    <!-- <Drawer/> -->
    <Header/>

    <div class="mt-6 ml-7 mr-14 flex justify-between">
      <h2 class="">All sneakers</h2>

      <div class="flex">
      <div class="relative">
        <img class="absolute left-3 top-3 w-4 h-5" src="/sort.svg">
        <select class="text-sm h-9 mr-8 w-48 border outline-none border-gray-300 rounded-sm focus:border-gray-400 transition pl-7">
          <option>Title</option>
          <option>Price (lower to higher)</option>
          <option>Price (higher to lower)</option>
        </select>
      </div>

      <div class="relative">
        <img class="absolute left-3 top-3" src="/search.svg">
        <input placeholder="Search.." class="text-sm pl-10 w-62 h-9 border outline-none border-gray-300 rounded-sm focus:border-gray-400 transition" type="text">
      </div>
      
    </div>
    </div>

      <CardList/>
    
  </div>
</template>



<style>

</style>