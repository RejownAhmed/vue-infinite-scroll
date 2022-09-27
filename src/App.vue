<template>
  <div>
    <ul>
      <li class="list-item" v-for="item in items" :key="item.id">{{item.name}}</li>
    </ul>
    <h1 v-show="loading" class="loading">Loading....</h1><!--Loader-->
    <Observer @intersect="fetchData"/>
  </div>
</template>

<script setup>
import Observer from './components/Observer.vue'
import { ref, onMounted } from "vue"

const page = ref(1)
const items = ref([])
const loading = ref(false)
//Just for demo purposes to show the loader working
const delay = (ms) => new Promise(resolve => setTimeout(resolve, ms)) //DEMO PURPOS ONLY
const fetchData = async () => {
  try {
    loading.value = true// Show loader
    const res = await fetch(`https://jsonplaceholder.typicode.com/comments?_page=${page.value}&_limit=50`);
    const newItems = await res.json();
    await delay(2000)
    items.value = [...items.value, ...newItems];
    page.value++
    loading.value = false //As all promise completed hide the loader
    return
  } catch(error){
    console.log("Erro Occured:" + error)
  }
}

onMounted(()=>{
  /*
  * This is needed for it to work properly
  * Because sometimes when route changes IntersectionObserver doesn't know the change and
  * does not load the initial data
  */
  fetchData() 
})
</script>

<style>

.loading{
  width: 100%;
  background-color: red;
  text-align: center;
  color: white
}
</style>