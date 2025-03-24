<script setup>
import { onMounted, ref} from 'vue';
import axios from 'axios'
import CardList from '../components/CardList.vue';

const favorites = ref([])

onMounted(async () => {
  try {
    const {data} = await axios.get('https://f5f2b5caba228578.mokky.dev/favorites')
    favorites.value = data.map((obj) => obj.item)
    console.log(favorites.value)
  } catch (error) {
    console.log(error)
  }
});

</script>


<template>
  <div class="mt-6 ml-7 mr-14 flex justify-between">
    <h1>Favorites</h1>
    <CardList :items="favorites" isFavoriteCard/>
  </div>
</template>
