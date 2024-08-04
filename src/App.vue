<script setup lang="ts">
import Treemap, { DataInterface } from './Treemap.vue';

const data: DataInterface[] = [
  {name: "Sport", value: 300, 
    children: [
      {name: "Sport 1", value: 200}, 
      {name: "Sport 2", value: 50}, 
      {name: "Sport 3", value: 50}
    ]
  },
  {name: "Santa Cruz", value: 50},
  {name: "Náutico", value: 200},
  {name: "Porto", value: 150},
  {name: "Central", value: 120},
  {name: "Salgueiro", value: 90},
];
</script>

<template>
  <main class="container">
    <Treemap :data="data" title="Pernambucano" :width="500" :height="500"/>
  </main>
</template>

<style scoped>
.container {
  width: 100vw;
  height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
}
</style>
