<script setup>
    defineProps({
        title: String,
        price: Number
    })

</script>

<template>
    <div class="border border-slate-200 p-2 rounded-xl flex relative gap-5 mt-4">
        <img class="w-22 h-22" src="/sneakers/sneakers-1.jpg">
        
        <div class="flex flex-col my-auto">
            <p class="text-sm">{{title}}</p>
            <p class="text-sm bold">Price: ${{ price }}</p>

            <img class="w-8 h-8 absolute right-0 top-0 m-3 opacity-60 transition hover:opacity-200 cursor-pointer" src="/close.svg">

        </div>
    </div>

</template>