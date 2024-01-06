<script setup>
import Home from '../components/Home.vue';
defineProps({
  msg: {
    type: String,
    required: true,
  },
});
</script>

<template>
  <Home />
</template>
