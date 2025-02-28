<script setup>
import Card from './components/Card.vue';
import CardList from './components/CardList.vue';
import Header from './components/Header.vue';
import Drawer from './components/Drawer.vue';

</script>

<template>
  <div class="text-2xl w-4/5 m-auto bg-white min-h-screen p-4 rounded-xl mt-20 shadow-xl">
    <Drawer/>
    <Header/>

      <div class="p-10">
      <h2 class="mt-6 mx-10">All sneakers</h2>
      <CardList/>

    </div>
  </div>
</template>



<style>

</style>