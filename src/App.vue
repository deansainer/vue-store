<script setup>
import CardList from './components/CardList.vue';
import Header from './components/Header.vue';
import axios from 'axios'
import { onMounted, ref, watch, reactive, provide} from 'vue';

// states
const items = ref([])
const favorites = ref([])

// applying filters for fetching items
const filters = reactive({
  sortBy: 'title',
  searchQuery: ''
})

// fetching items
async function fetchItems() {
  try {
    const { data } = await axios.get('https://f5f2b5caba228578.mokky.dev/items', {
      params: {
        sortBy: filters.sortBy,
        title: filters.searchQuery ? `*${filters.searchQuery}*` : null
      }
    });

    items.value = data.map((obj) => ({
      ...obj,
      isFavorite: false,
      isAdded: false
    }))

  } catch (error) {
    console.log(error);
  }
}

// fetching favorites
async function fetchFavorites() {
  try {
    const { data } = await axios.get('https://f5f2b5caba228578.mokky.dev/favorites');
    favorites.value = data;

    // iterate over all items in items.value and check if they exist in favorites.value
    items.value = items.value.map((item) => {
      const favorite = favorites.value.find(fav => fav.parentId === item.id);    //find the item in the favorites list using its parentId
      
      // If the item is found in favorites, mark it as "isFavorite"
      return favorite
        ? { ...item, isFavorite: true, favoriteId: favorite.id }
        : item;
    });
    console.log(items.value)
  } catch (error) {
    console.log(error);
  }
}

// rendering once page is opened
onMounted(async () => {
  await fetchItems();
  await fetchFavorites();
});

// listening to changes in filters & executing fetchItems func when changed
watch(filters, fetchItems)
 
function onChangeSelect(event){
  filters.sortBy = event.target.value
}

function onChangeSearchQuery(event){
  filters.searchQuery = event.target.value
}


function addToFavorites(item){
  item.isFavorite = !item.isFavorite
}

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
        <select @change="onChangeSelect" class="text-sm h-9 mr-8 w-48 border outline-none border-gray-300 rounded-sm focus:border-gray-400 transition pl-7">
          <option value="title">Title</option>
          <option value="price">Price (lower to higher)</option>
          <option value="-price">Price (higher to lower)</option>
        </select>
      </div>

      <div class="relative">
        <img class="absolute left-3 top-3" src="/search.svg">
        <input @input="onChangeSearchQuery" placeholder="Search.." class="text-sm pl-10 w-62 h-9 border outline-none border-gray-300 rounded-sm focus:border-gray-400 transition" type="text">
      </div>
      
    </div>
    </div>

      <CardList :items="items" @addToFavorites="addToFavorites"/>
    
  </div>
</template>



<style>

</style>