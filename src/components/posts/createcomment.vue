<script setup>
import { ref } from 'vue';
import { useRoute } from 'vue-router';
import axios from 'axios';

const route = useRoute();
const stuff = ref('');
const respond = ref(null);
const emits = defineEmits(['successful']);

async function post() {
  let session = window.sessionStorage.getItem("session");
  if (session) {
    let response = await axios.post(`/api/newcomment/${route.params.id}`, {
      stuff: stuff,
      session: session
    })
    if (response.data.includes("successful")) {
      emits("successful");
    } else respond.value = response.data;
  } else respond.value = "Please login";
}
</script>
<template>
  <div class="flex flex-col m-2 w-auto p-4 pt-0 bg-nav-bg rounded-lg justify-center">
    <h1 class="text-3xl m-2 base_color_text text-center font-poppins">Create a comment</h1>
    <h2 class="text-red-600 m-2 text-center" v-if="respond">{{ respond }}</h2>
    <form @submit.prevent="post">
      <div>
        <textarea required v-model="stuff"
          class="h-35 w-full text-lg outline-blue-500 outline-8 text-start"
          placeholder=""></textarea>
      </div>
      <button
        class="base_background font-poppins text-2xl text-center text-white h-10 w-full">Submit</button>
    </form>
  </div>
</template>