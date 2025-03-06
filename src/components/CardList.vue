<script setup>
import CardItem from './CardItem.vue'

defineProps({
  items: Array,
})

const emit = defineEmits(['addToFavorites', 'addToCart', 'items'])

</script>

<template>
  <div class="grid grid-cols-4 gap-5 ml-5 mr-2">
    <CardItem
      v-for="item in items"
      :key="item.id"
      :id="item.id"
      :isAdded="item.isAdded"
      :isFavorite="item.isFavorite"
      :title="item.title"
      :price="item.price"
      :imageUrl="item.imageUrl"
      :addToFavorites="() => emit('addToFavorites', item)"
      :addToCart="() => emit('addToCart', item)"
    />
  </div>
</template>
