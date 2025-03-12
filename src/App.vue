<script setup>
import CardList from './components/CardList.vue'
import Header from './components/Header.vue'
import axios from 'axios'
import { onMounted, ref, watch, computed, reactive, provide } from 'vue'
import Drawer from './components/Drawer.vue'
import InfoBlock from './components/InfoBlock.vue'

// states
const items = ref([])
const favorites = ref([])
const isDrawerOpen = ref(false)
const cartItems = ref([])

// applying filters for fetching items
const filters = reactive({
  sortBy: 'title',
  searchQuery: '',
})

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

// fetching items
async function fetchItems() {
  try {
    const { data } = await axios.get('https://f5f2b5caba228578.mokky.dev/items', {
      params: {
        sortBy: filters.sortBy,
        title: filters.searchQuery ? `*${filters.searchQuery}*` : null,
      },
    })

    items.value = data.map((obj) => ({
      ...obj,
      isFavorite: false,
      isAdded: false,
    }))
  } catch (error) {
    console.log(error)
  }
}

// fetching favorites
async function fetchFavorites() {
  try {
    const { data } = await axios.get('https://f5f2b5caba228578.mokky.dev/favorites')
    favorites.value = data

    // update items with favorites status
    items.value = items.value.map((item) => {
      const favorite = favorites.value.find((fav) => fav.parentId === item.id)
      return favorite
        ? { ...item, isFavorite: true, favoriteId: favorite.id, isAdded: false }
        : item
    })
  } catch (error) {
    console.log(error)
  }
}

// creating order
async function createOrder() {
  try {
    await axios.post('https://f5f2b5caba228578.mokky.dev/orders', {
      cartTotal: cartTotal.value,
      items: cartItems.value,
    })
    cartItems.value.forEach((item) => item.isAdded = false)
    cartItems.value = [] // Clear the cart
  } catch (error) {
    console.log(error)
  }
}

// rendering once page is opened
onMounted(async () => {
  await fetchItems()
  await fetchFavorites()
})

// listening to changes in filters & executing fetchItems func when changed
watch(filters, fetchItems)


function onChangeSelect(event) {
  filters.sortBy = event.target.value
}

function onChangeSearchQuery(event) {
  filters.searchQuery = event.target.value
}

// favorites handling
async function addToFavorites(item) {
  try {
    if (!item.isFavorite) {
      const obj = { parentId: item.id }
      item.isFavorite = true

      const { data } = await axios.post('https://f5f2b5caba228578.mokky.dev/favorites', obj)
      item.favoriteId = data.id
    } else {
      item.isFavorite = false
      await axios.delete(`https://f5f2b5caba228578.mokky.dev/favorites/${item.favoriteId}`)
      item.favoriteId = null
    }
  } catch (error) {
    console.log(error)
  }
}

// cart handling
function onClickAddToCart(item) {
  if (!item.isAdded) {
    addToCart(item)
  } else {
    removeFromCart(item)
  }
}

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
    <!-- empty basket block <InfoBlock/> -->
    <div class="mt-6 ml-7 mr-14 flex justify-between">
      <h2>All sneakers</h2>

      <div class="flex">
        <div class="relative">
          <img class="absolute left-3 top-3 w-4 h-5" src="/sort.svg" />
          <select
            @change="onChangeSelect"
            class="text-sm h-9 mr-8 w-48 border outline-none border-gray-300 rounded-sm focus:border-gray-400 transition pl-7"
          >
            <option value="title">Title</option>
            <option value="price">Price (low to high)</option>
            <option value="-price">Price (high to low)</option>
          </select>
        </div>

        <div class="relative">
          <img class="absolute left-3 top-3" src="/search.svg" />
          <input
            @input="onChangeSearchQuery"
            placeholder="Search.."
            class="text-sm pl-10 w-62 h-9 border outline-none border-gray-300 rounded-sm focus:border-gray-400 transition"
            type="text"
          />
        </div>
      </div>
    </div>

    <CardList
      @onClickAddToCart="onClickAddToCart"
      :items="items"
      @addToFavorites="addToFavorites"
    />
  </div>
</template>
