<script setup>
import CartItem from './CartItem.vue';

</script>

<template>
    <div>
        <CartItem title="Nike Air Force" :price="49.99"/>
        <CartItem title="Asics Gel 2" :price="59.99"/>
        <CartItem title="New Balance 530" :price="69.99"/>
    </div>
  </template>
  