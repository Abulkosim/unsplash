<template>
  <div class="container mx-auto p-8">
    <div class="max-w-4xl mx-auto relative border-2 border-gray-300 rounded-[20px] ">
      <input type="text" placeholder="Search users..."
        class="w-full h-full p-5 pr-14 text-2xl rounded-[20px] text-gray-500 selection:bg-gray-500 selection:text-white outline-none placeholder:text-gray-400"
        v-model="query" @input="searchPhotos">
      <img src="../assets/search.svg" alt="Search" class="absolute top-4 right-3 cursor-pointer select-none p-1"
        @click="searchPhotos">
    </div>
    <div class="max-w-5xl mx-auto grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-2 my-8 text-gray-500">
      <div v-for="user in users"
        class="flex flex-col items-center justify-center text-center gap-1  border border-gray-200 shadow p-5 rounded-xl hover:bg-gray-100 overflow-x-auto">
        <div class="rounded-full shadow-lg mb-2">
          <img :src="user.profile_image.large" alt="" class="rounded-full">
        </div>
        <span class="text-2xl font-semibold">{{ user.name }}</span>
        <a :href="user.links.html">
          <span class="text-lg">{{ `Username: ${user.username}` }}</span>
        </a>
        <span class="text-lg" v-if="user.location">{{ `From: ${user.location}` }}</span>
        <span class="text-lg">{{ `Total likes: ${user.total_likes}` }}</span>
        <span class="text-lg">{{ `Total photos: ${user.total_photos}` }}</span>
        <p class="italic" v-if="user.bio" :title="user.bio">{{ user.bio }}</p>
        <div class="flex items-center gap-4 mt-2">
          <a :href="`https://www.x.com/${user.twitter_username}`" target="_blank"
            class="hover:text-gray-600 transition ease-in-out delay-150" v-if="user.twitter_username">
            <ph-twitter-logo :size="32" weight="fill" />
          </a>
          <a :href="`https://www.instagram.com/${user.instagram_username}`" target="_blank"
            class="hover:text-gray-600 transition ease-in-out delay-150" v-if="user.instagram_username">
            <ph-instagram-logo :size="32" weight="fill" />
          </a>
          <a :href="user.portfolio_url" target="_blank" class="hover:text-gray-600 transition ease-in-out delay-150"
            v-if="user.portfolio_url">
            <ph-bag :size="32" weight="fill" />
          </a>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import axios from 'axios'

import { PhTwitterLogo, PhInstagramLogo, PhBag } from "@phosphor-icons/vue";

const query = ref('')
const url = 'https://api.unsplash.com/';
const key = 'client_id=u9fK2wHosa-xHNkpqldYTnYZrDBiLObD3kCln1Vq7h8';
const users = ref()

async function searchPhotos() {
  const response = await axios.get(`${url}search/users?per_page=30&query=${query.value}&${key}`)
  users.value = response.data.results
}
</script>