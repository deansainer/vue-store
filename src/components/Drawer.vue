<script setup>
import { inject } from 'vue';
import CartItemList from './CartItemList.vue';
import InfoBlock from './InfoBlock.vue';

const {openDrawer, closeDrawer} = inject('cart')

defineProps({
    cartTotal: Number,
    createOrder: Function
})

</script>


<template>
    <div>
        <div  @click="closeDrawer"  class="bg-black fixed top-0 left-0 h-full w-full opacity-70 z-10"></div>
        <div class="bg-white flex h-full fixed top-0 right-0 w-96 h-full z-20 p-5 flex-col">
            <InfoBlock v-if="cartTotal <= 0"/>

            <div class="h-full flex flex-col" v-if="cartTotal > 0">
                <div  class="flex items-center gap-5 h-10">
                    <img @click="closeDrawer" class="w-8 h-8 cursor-pointer mt-1 opacity-40 hover:opacity-90 transition" src="/arrow-back.svg">
                    <span class="text-2xl">Cart</span>
                </div>
                
                <CartItemList/>
                
                <div class="mt-auto">
                    <div class="flex justify-between">
                        <span>Total: </span>
                        <span>${{cartTotal}}</span>
                    </div>

                    <div class="flex justify-between">
                        <span>Fee 5%: </span>
                        <span>${{(cartTotal/20).toFixed(2)}}</span>
                    </div>

                    <button :disabled="cartTotal <= 0" @click="createOrder" class="text-white mt-4 bg-sky-800 hover:bg-sky-900 active:bg-sky-950 disabled:bg-slate-300 disabled:cursor-default transition py-3 cursor-pointer w-full rounded-xl">
                        Order
                    </button>
                </div>
            </div>

        </div>
    </div>
</template>

