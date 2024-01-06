<script setup>
import Card from './Card.vue';
import Navbar from './Navbar.vue';
defineProps({
  msg: {
    type: String,
    required: true,
  },
});
</script>

<template>
  <Card />
</template>
