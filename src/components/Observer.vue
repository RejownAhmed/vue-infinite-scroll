<template>
    <div ref="root"></div>
  </template>
  
  <script setup>
    import { onMounted, onUnmounted, ref } from 'vue';
    const props = defineProps(
        {
            options: {
                type: Object,
                default: {}
            }
        }
    )
    const root = ref(null)
    const observer = ref()
    const emit = defineEmits(['intersect'])
    onMounted(() => {
      observer.value = new IntersectionObserver(([entry]) => {
        if (entry && entry.isIntersecting) {
          emit("intersect");
        }
      }, props.options);
  
      observer.value.observe(root.value);
    })
    onUnmounted(() => {
      observer.value.disconnect();
    })
  </script>