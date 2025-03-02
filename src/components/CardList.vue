<script setup>
import CardItem from './CardItem.vue'


defineProps({
  items: Array,
  addToFavorites: Function
})

const emit = defineEmits(['addToFavorites'])

</script>

<template>
  <div class="grid grid-cols-4 gap-5 ml-5 mr-2">
    <CardItem
      v-for="item in items"
      :key="item.id"
      :id="item.id"
      :isAdded="false"
      :isFavorite="item.isFavorite"
      :title="item.title"
      :price="item.price"
      :imageUrl="item.imageUrl"
      :addToFavorites="() => emit('addToFavorites', item)"
    />
  </div>
</template>
