<script setup>
import { inject, reactive, watch, ref, onMounted } from 'vue';
import CardList from '../components/CardList.vue';
import axios from 'axios'
import InfoBlock from '../components/InfoBlock.vue';

const {addToCart, cartItems, removeFromCart} = inject('cart')

const favorites = ref([])
const items = ref([])

// applying filters for fetching items
const filters = reactive({
  sortBy: 'title',
  searchQuery: '',
})

function onChangeSelect(event) {
  filters.sortBy = event.target.value
}

function onChangeSearchQuery(event) {
  filters.searchQuery = event.target.value
}


function onClickAddToCart(item) {
  if (!item.isAdded) {
    addToCart(item)
  } else {
    removeFromCart(item)
  }
}

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

// rendering once page is opened
onMounted(async () => {
  const localCartItems = localStorage.getItem('cartItems')
  cartItems.value = localCartItems ? JSON.parse(localCartItems) : []
  
  await fetchItems()
  await fetchFavorites()


  items.value = items.value.map((item) => ({
    ...item,
    isAdded: cartItems.value.some((cartItem) => cartItem.id === item.id)
  }))
})

// listening to changes in filters & executing fetchItems func when changed
watch(filters, fetchItems)

// saving items into local storage
watch(cartItems, () => {
  localStorage.setItem('cartItems', JSON.stringify(cartItems.value))
},
  {deep: true}
)

</script>

<template>
  <div>
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
