<script setup lang="ts">
import Treemap, { type DataInterface } from './Treemap.vue';

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

const stockMarketData: DataInterface[] = [
  { 
    name: "Tecnologia", 
    value: 1200, 
    children: [
      { 
        name: "Apple", 
        value: 400, 
        children: [
          { name: "Hardware", value: 200 },
          { name: "Software", value: 100 },
          { name: "Serviços", value: 100 }
        ] 
      },
      { 
        name: "Microsoft", 
        value: 350, 
        children: [
          { name: "Windows", value: 150 },
          { name: "Azure", value: 100 },
          { name: "Office", value: 100 }
        ] 
      },
      { 
        name: "Google", 
        value: 450, 
        children: [
          { name: "Pesquisa", value: 200 },
          { name: "YouTube", value: 150 },
          { name: "Cloud", value: 100 }
        ] 
      }
    ]
  },
  { 
    name: "Saúde", 
    value: 800, 
    children: [
      { 
        name: "Pfizer", 
        value: 300, 
        children: [
          { name: "Vacinas", value: 150 },
          { name: "Medicamentos", value: 100 },
          { name: "Pesquisa", value: 50 }
        ] 
      },
      { 
        name: "Johnson & Johnson", 
        value: 250, 
        children: [
          { name: "Farmacêutica", value: 100 },
          { name: "Produtos de consumo", value: 100 },
          { name: "Dispositivos médicos", value: 50 }
        ] 
      },
      { 
        name: "Moderna", 
        value: 250, 
        children: [
          { name: "Vacinas", value: 150 },
          { name: "Pesquisa", value: 50 },
          { name: "Desenvolvimento", value: 50 }
        ] 
      }
    ]
  },
  { 
    name: "Energia", 
    value: 700, 
    children: [
      { 
        name: "ExxonMobil", 
        value: 300, 
        children: [
          { name: "Petróleo", value: 150 },
          { name: "Gás Natural", value: 100 },
          { name: "Exploração", value: 50 }
        ] 
      },
      { 
        name: "Chevron", 
        value: 250, 
        children: [
          { name: "Petróleo", value: 150 },
          { name: "Gás Natural", value: 50 },
          { name: "Refinamento", value: 50 }
        ] 
      },
      { 
        name: "NextEra Energy", 
        value: 150, 
        children: [
          { name: "Energia Solar", value: 100 },
          { name: "Energia Eólica", value: 50 }
        ] 
      }
    ]
  }
];


</script>

<template>
  <main class="container">
      <Treemap :data="data" title="Pernambucano" :width="500" :height="500"/>
      <Treemap :data="animalData" title="Animaias" :width="1000" :height="1000"/>
      <Treemap :data="stockMarketData" title="stockMarketData" :width="900" :height="800"/>
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
