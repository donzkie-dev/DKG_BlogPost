<script setup>
import { onMounted, ref } from 'vue';
import { useRoute, useRouter } from 'vue-router';
import axios from 'axios';
import { marked } from 'marked';
import DOMPurify from 'dompurify';

const router = useRouter();
const route = useRoute();
const items = ref({ title: "", description: "", owner: "" });
const show = ref(null);
const error = ref(null);

function notdone() {
  alert("Not Done")
};
function scroll(id) {
  document.getElementById(id).scrollIntoView({ behavior: 'smooth' });
};
function showthing(id) {
  show.value = id;
};
function profile(user) {
  router.push(`/profile/${user}`);
};

async function get() {
  let response = await axios.get(`/api/data/comment/${route.params.id}`)
  if (response.data) {
    items.value = response.data;
  } else error.value = "Not found";
};
onMounted(get)
</script>

<template>
  <div class="base_background text-center p-5">
    <h1 v-if="error" class="text-red-700 text-3xl">{{ error }}</h1>
    <h1 class="text-white text-3xl">{{ items.title }}</h1>
  </div>
  <div @mouseenter="showthing(items._id)" @mouseleave="showthing(null)"
    class="bg-nav-bg p-4 rounded-lg m-2 text-white overflow-auto relative">
    <h2 @click="profile(items.owner)" class="hover:cursor-pointer">{{ `${items.owner}:` }}</h2>
    <div class="whitespace-pre" v-html="DOMPurify.sanitize(marked.parse(items.description))"></div>
  </div>

  <div v-for="item in items.comments" :id="item._id" @mouseenter="showthing(item._id)" @mouseleave="showthing(null)"
    class="bg-nav-bg p-4 rounded-lg m-2 text-white overflow-auto relative">
    <h2 @click="profile(item.owner)" class="hover:cursor-pointer">{{ `${item.owner}:` }}</h2>
    <div class="whitespace-pre" v-html="DOMPurify.sanitize(marked.parse(item.stuff))"></div>
  </div>
</template>