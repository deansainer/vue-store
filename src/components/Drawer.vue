<script setup>
import { inject } from 'vue';
import CartItemList from './CartItemList.vue';
const {openDrawer, closeDrawer} = inject('cart')

defineProps({
    
})
</script>


<template>
    <div>
        <div  @click="closeDrawer"  class="bg-black fixed top-0 left-0 h-full w-full opacity-70 z-10"></div>
        <div class="bg-white flex h-full fixed top-0 right-0 w-96 h-full z-20 p-5 flex-col">
            <div  class="flex items-center gap-5 h10">
                <img @click="closeDrawer" class="w-8 h-8 cursor-pointer mt-1 opacity-40 hover:opacity-90 transition" src="/arrow-back.svg">
                <span class="text-2xl">Cart</span>
            </div>
            
            <CartItemList/>
            
            <div class="mt-auto mx-1">
                <div class="flex justify-between">
                    <span>Total: </span>
                    <span>$246.92</span>
                </div>

                <div class="flex justify-between">
                    <span>Fee 5%: </span>
                    <span>$11.92</span>
                </div>

                <button class="text-white mt-4 bg-sky-800 hover:bg-sky-900 active:bg-sky-950 disabled:bg-slate-300 transition py-3 cursor-pointer w-full rounded-xl">
                    Order
                </button>

            </div>

        </div>
    </div>
</template>

