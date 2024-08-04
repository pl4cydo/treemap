<script setup lang="ts">
import Treemap, { DataInterface } from './Treemap.vue';

const data: DataInterface[] = [
  {name: "Sport", value: 300, 
    children: [
      {name: "Sport 1", value: 190}, 
      {name: "Sport 2", value: 60}, 
      {name: "Sport 3", value: 50}
    ]
  },
  {name: "Santa Cruz", value: 200,
    children: [
      {name: "Santa Cruz 1", value: 120}, 
      {name: "Santa 2", value: 60}, 
      {name: "Santa 3", value: 20}, 
    ]
  },
  {name: "Náutico", value: 100},
  {name: "Porto", value: 150},
  {name: "Central", value: 120},
  {name: "Salgueiro", value: 90},
];

const animalData: DataInterface[] = [
  { 
    name: "Mamíferos", 
    value: 500, 
    children: [
      { name: "Leão", value: 100 },
      { name: "Elefante", value: 200 },
      { name: "Macaco", value: 200 }
    ]
  },
  { 
    name: "Aves", 
    value: 300, 
    children: [
      { name: "Águia", value: 100 },
      { name: "Papagaio", value: 150 },
      { name: "Pombo", value: 50 }
    ]
  },
  { 
    name: "Répteis", 
    value: 200, 
    children: [
      { name: "Cobra", value: 120 },
      { name: "Jacaré", value: 80 }
    ]
  },
  { 
    name: "Peixes", 
    value: 150, 
    children: [
      { name: "Tubarão", value: 80 },
      { name: "Salmão", value: 70 }
    ]
  },
  { 
    name: "Anfíbios", 
    value: 100, 
    children: [
      { name: "Rã", value: 50 },
      { name: "Salamandra", value: 50 }
    ]
  }
];
</script>

<template>
  <main class="container">
      <Treemap :data="data" title="Pernambucano" :width="500" :height="500"/>
      <Treemap :data="animalData" title="Animaias" :width="1000" :height="1000"/>
  </main>
</template>

<style scoped>
.container {
  width: 100vw;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
}
.feed {
  width: 80%;
  height: auto;
}
</style>
