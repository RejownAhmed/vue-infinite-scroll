<template>
  <div>
    <ul>
      <li class="list-item" v-for="item in items" :key="item.id">{{item.name}}</li>
    </ul>
    <Observer @intersect="intersected"/>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import Observer from "./components/Observer.vue"

const page = ref(1)
const items = ref([])
const intersected = async () => {
    const res = await fetch(`https://jsonplaceholder.typicode.com/comments?_page=${page.value}&_limit=50`);

    page.value++;
    const newItems = await res.json();
    items.value = [...items.value, ...newItems];
  }
</script>