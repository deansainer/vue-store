<script setup>
import { inject } from 'vue';


const {openDrawer, closeDrawer} = inject('cart')

const props = defineProps({
    title: String,
    description: String,
    imageURL: String
})

</script>

<template>
    <div>
        <div  class="flex items-center gap-5 h-10">
            <img @click="closeDrawer" class="w-8 h-8 cursor-pointer mt-1 opacity-40 hover:opacity-90 transition" src="/arrow-back.svg">
            <span class="text-2xl">Cart</span>
        </div>

    <div class="flex text-center flex-col mt-70 items-center h-full w-full">
        <img class="opacity-40" height="150" width="150" alt="infoImage" :src="props.imageURL">
        <h2 class="text-slate-400 mt-4">{{ props.title }}</h2>
        <p class="text-slate-400">{{ props.description }}</p>
    </div>
    
</div>
</template>