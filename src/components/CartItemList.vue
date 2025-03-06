<script setup>
import CartItem from './CartItem.vue';
import { inject } from 'vue';

const {cartItems, removeFromCart} = inject('cart')

</script>

<template>
    <div>
        <CartItem v-for="item in cartItems"
        :key="item.id"
        :title="item.title"
        :price="item.price"
        :isFavorite="item.isFavorite"
        :isAdded="item.isAdded"
        :imageUrl="item.imageUrl"
        @removeFromCart="removeFromCart(item)"
        />
    </div>
  </template>
  