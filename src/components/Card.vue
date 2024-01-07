<script setup>
const data = [
  {
    name: 'Spongebob 1',
    desc: 'Spongebob template',
    imageName: 'spongebob1.png',
    image: new URL('../assets/images/spongebob1.png', import.meta.url).href,
  },
  {
    name: 'Sonic 1',
    desc: 'Sonic template',
    imageName: 'sonic1.jpg',
    image: new URL('../assets/images/sonic1.jpg', import.meta.url).href,
  },
];
</script>

<template>
  <div class="mx-auto p-4">
    <div class="grid grid-cols-4 gap-4">
      <div
        class="card bg-base-100 shadow-xl"
        v-for="(item, index) in data"
        :key="index"
      >
        <figure>
          <img class="h-60" :src="item.image" />
        </figure>
        <div class="card-body">
          <h2 class="card-title">{{ item.name }}</h2>
          <p>{{ item.desc }}</p>
          <div class="card-actions justify-end">
            <RouterLink
              :to="{
                name: 'editor',
                params: { id: item.imageName },
              }"
              class="btn btn-primary"
              >Use template</RouterLink
            >
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
