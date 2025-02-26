<script>
import Card from './components/Card.vue';
import Header from './components/Header.vue';

export default {
  components: {
    Header,
    Card
  }
};
</script>

<template>
  <div class="text-2xl w-4/5 m-auto bg-white min-h-screen p-4 rounded-xl mt-20 shadow-xl">
      <Header/>
      <Card/>

  </div>
</template>



<style>

</style>