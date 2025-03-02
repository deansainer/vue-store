<script setup>

    defineProps({
      id: Number,
      imageUrl: String,
      title: String,
      price: Number,
      isFavorite: Boolean,
      isAdded: Boolean,
      addToFavorites: Function
    })

</script>

<template>
    <div>
      <div class="bg-white mt-6 h-90 p-6 w-76 border border-slate-200 relative rounded-xl shadow-lg cursor-pointer transition hover:-translate-y-2 hover:shadow-xl">
        <img @click="addToFavorites" class="absolute right-5 bottom-6" :src="isFavorite ? '/like-2.svg' : '/like-1.svg'" alt="like2">
        <img :src="imageUrl" class="w-46 mx-auto" alt="sneaker">
        <p class="text-xl px-2 mt-4">{{ title }}</p>

        <div class="flex justify-between px-2 mt-4">
          <div class="flex flex-col absolute bottom-4">
            <span class="text-slate-400 text-base">Price</span>
            <b class="text-lg">${{ price }}</b>
          </div>

          <img class="absolute right-5 top-4" @click="onClickAdd" :src="isAdded ? '/checked.svg' : '/plus.svg'" alt="plus"/>
        </div>

      </div>
    </div>
</template>