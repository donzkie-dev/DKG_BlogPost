<script setup>
import { ref } from 'vue';
import axios from 'axios';

const title = ref('');
const description = ref('');
const respond = ref(null)
const emits = defineEmits(['successful']);

async function post() {
  let session = window.sessionStorage.getItem("session")
  if (session) {
    let response = await axios.post("/api/newpost", {
      title: title,
      description: description,
      session: session
    })
    if (response.data.includes("successful")) {
      emits("successful");
    } else respond.value = response.data;
  } else respond.value = "Please login";
}
</script>
<template>
  <div class="h-half flex justify-center items-center flex-col m-24">
    <div class="bg-nav-bg p-2 py-0 rounded-xl">
      <h1 class="text-3xl m-6 base_color_text text-center font-poppins">Create a post</h1>
      <h2 class="text-red-600 m-2 text-center" v-if="respond">{{ respond }}</h2>
      <form @submit.prevent="post">
        <div>
          <input required v-model="title"
            class="m-2 w-buttonr h-10 text-2xl outline-blue-500 outline-8 text-start indent-1"
            placeholder="Title">
        </div>
        <div>
          <textarea required v-model="description"
            class="m-2 w-buttonr h-58 text-lg outline-blue-500 outline-8 text-start"
            placeholder="Description"></textarea>
        </div>
        <button
          class="m-2 base_background font-poppins text-2xl text-center text-white w-buttonr h-10">Submit</button>
      </form>
    </div>
  </div>
</template>