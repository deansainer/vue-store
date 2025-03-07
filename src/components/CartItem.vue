<script setup>
    const emit = defineEmits(['removeFromCart'])
    defineProps({
        id: Number,
        imageUrl: String,
        title: String,
        price: Number,
        isFavorite: Boolean,
        isAdded: Boolean,
        removeFromCart: Function
    })

</script>

<template>
    <div class="border border-slate-200 p-2 rounded-xl flex relative gap-5 mt-4">
        <img class="w-22 h-22" :src="imageUrl">
        
        <div class="flex flex-col my-auto">
            <p class="text-sm">{{title}}</p>
            <p class="text-sm bold">Price: ${{ price }}</p>

            <img @click="emit('removeFromCart')" class="w-8 h-8 absolute right-0 top-0 m-3 opacity-60 transition hover:opacity-200 cursor-pointer" src="/close.svg">

        </div>
    </div>

</template>