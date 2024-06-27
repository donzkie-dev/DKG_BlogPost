<script setup>
import { onMounted, ref } from 'vue';
import { useRouter } from 'vue-router';
import axios from 'axios';

const router = useRouter();
const items = ref([]);

async function getPostData(){
  let response = await axios.get("/api/data/post")
  if(response != null || undefined) {
  items.value = response.data;
  }
};

function go(id){
  router.push({ path: `/post/${id}` });
};

onMounted(getPostData);
</script>
<template>
  <div v-for="item in items" @click="go(item._id)" class="bg-nav-bg m-4 p-2 text-white w-3/4">
    <h3>{{ `${item.owner}:` }}</h3>
    <h1>{{ item.title }}</h1>
  </div>
</template>