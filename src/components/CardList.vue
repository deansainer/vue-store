<script setup>
import CardItem from './CardItem.vue'

defineProps({
  items: Array,
})

function onClickAdd() {
  alert('Added to cart')
}

function onClickFavorite() {
  alert('Added to favorites!')
}
</script>

<template>
  <div class="grid grid-cols-4 gap-5 ml-5 mr-2">
    <CardItem
      v-for="item in items"
      :key="item.id"
      :onClickFavorite="onClickFavorite"
      :onClickAdd="onClickAdd"
      :isAdded="false"
      :isFavorite="item.isFavorite"
      :title="item.title"
      :price="item.price"
      :imageUrl="item.imageUrl"
    />
  </div>
</template>
