<script setup>
import Card from './Card.vue'

function onClickAdd() {
  alert('Added to cart')
}

function onClickFavorite() {
  alert('Added to favorites!')
}
</script>

<template>
  <div class="grid grid-cols-4 gap-5">
    <Card
      :onClickFavorite="onClickFavorite"
      :onClickAdd="onClickAdd"
      :isAdded="false"
      title="Nike Blazer Mid"
      :price="89.99"
      imageUrl="/sneakers/sneakers-1.jpg"
    />
  </div>
</template>
