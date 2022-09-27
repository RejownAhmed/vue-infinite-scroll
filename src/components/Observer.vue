<template>
    <div ref="root"></div>
  </template>
  
  <script setup>
    import { onMounted, onUnmounted, ref } from 'vue';
    const props = defineProps(
        {
            options: Object,
        }
    )
    const root = ref(null)
    const emit = defineEmits(['intersect'])
    onMounted(() => {
      console.log(root.value.outerHTML)
      const options = props.options || {};
      const observer = new IntersectionObserver(([entry]) => {
        if (entry && entry.isIntersecting) {
          emit("intersect");
        }
      }, props.options);
  
      observer.observe(root.value);
    })
    onUnmounted(() => {
      observer.disconnect();
    })
  </script>