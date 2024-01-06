<script setup>
const data = [
  {
    name: 'Lisa',
    desc: 'Member Blackpink',
    image: 'lisa',
  },
  {
    name: 'Jiso',
    desc: 'Member Blackpink',
    image: 'jiso',
  },
  {
    name: 'Rose',
    desc: 'Member Blackpink',
    image: 'rose',
  },
  {
    name: 'Jennie',
    desc: 'Member Blackpink',
    image: 'jennie',
  },
];
function editAction() {}
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
          <img
            class="h-60"
            :src="'../src/assets/images/' + item.image + '.jpg'"
          />
        </figure>
        <div class="card-body">
          <h2 class="card-title">{{ item.name }}</h2>
          <p>{{ item.desc }}</p>
          <div class="card-actions justify-end">
            <RouterLink
              :to="{
                name: 'editor',
                params: { id: item.image },
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
