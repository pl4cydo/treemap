<script setup lang="ts">
import { onMounted, ref, type Ref } from 'vue';

export interface DataInterface {
  name: string,
  value: number,
  children?: DataInterface[]
}

const props = defineProps<{
    data: DataInterface[],
    title: string,
    width: number,
    height: number
}>()

type RectangleType = {
  top: string, 
  left: string, 
  height: string, 
  width: string, 
  backgroundColor: string,
  border: string,
  position: string,
  display?: string
  name?: string,
  value?: number
};

interface TreemapDataInterface {
  squere: RectangleType,
  rectangles: RectangleType[]
}

interface boundsInterface { 
  startTop: number, 
  startLeft: number, 
  right: number, 
  bottom: number 
};

let treemapData: Ref<TreemapDataInterface> = ref({
    squere: { 
      top: '0px', 
      left: '0px', 
      height: '0px', 
      width: '0px', 
      backgroundColor: 'white',
      border: '1px solid black',
      position: 'relative',
    },
    rectangles: []
  })

function treemap(data: DataInterface[], width: number, height: number, y: number = 0, x: number = 0): void {
  if (!data || data.length === 0) return;
  // organiza o array
  data.sort((a, b) => b.value - a.value)
  //soma os valores do array
  const sum: number = data.reduce((s, i) => s + i.value, 0);
  // salva as coordenadas do espaço vazio
  const bounds: boundsInterface = { startTop: y, startLeft: x, right: width, bottom: height };
  // quanto falta dos valores 
  let weightLeft: number = sum;
  // declar os eixos e tamanhos do retangulo a ser gerado
  let eixoX: number, eixoY: number, rectangleWidth: number, rectangleHeigth: number;

  // declara a cor inical retangulo
  let color1: number = 0;
  let color2: number = 170;
  let color3: number = 0;

  treemapData.value.squere = { 
      top: `${bounds.startTop}px`, 
      left: `${bounds.startLeft}px`, 
      height: `${bounds.bottom}px`, 
      width: `${bounds.right}px`, 
      backgroundColor: `white`,
      border: '1px solid black',
      position: 'relative',
  },

  // itera pelo array de numeros e gera o retangulo
  data.forEach(el => {
    const horizontalVoidSpace: number = bounds.right - bounds.startLeft;
    const verticalVoidSpace: number = bounds.bottom - bounds.startTop;
    // const area = el / unit;
    eixoX = bounds.startLeft;
    eixoY = bounds.startTop;
    if (horizontalVoidSpace > verticalVoidSpace) {
      // horizontalmente
      rectangleWidth = (el.value / weightLeft) * horizontalVoidSpace;
      rectangleHeigth = verticalVoidSpace;
      bounds.startLeft = eixoX + rectangleWidth;
    } else {
      // verticalmente
      rectangleWidth = horizontalVoidSpace;
      rectangleHeigth = (el.value / weightLeft) * verticalVoidSpace;
      bounds.startTop = eixoY + rectangleHeigth;
    }

    weightLeft -= el.value;

    treemapData.value.rectangles.push({ 
      name: el.name,
      top: `${eixoY}px`,
      left: `${eixoX}px`, 
      height: `${rectangleHeigth}px`, 
      width: `${rectangleWidth}px`,
      backgroundColor: `rgb(${color1}, ${color2}, ${color3})`,
      border: '1px solid white',
      position: 'absolute',
      value: el.value
    })

    color1+= 10;
    color2+= 10;

    if (el.children && el.children.length > 0) {
        treemap(el.children, rectangleWidth, rectangleHeigth, eixoY, eixoX)
    }
  });

  console.log(treemapData.value)
}

onMounted(() => {
  treemap(props.data, props.width, props.height)
});

</script>

<template>
    <div style="height: 100vh; width: 100vw;">
        <div :style="treemapData.squere">
          <div 
            v-for="rectangle in treemapData.rectangles" 
            :style="{
              ...rectangle, 
              display: 'flex', 
              alignItems: 'center', 
              justifyContent: 'center', 
              overflow: 'hidden',
              flexDirection: 'column'
              }">
              <span>{{ rectangle.name }}</span>
              <span>{{ rectangle.value }}</span>
          </div>
        </div>
    </div>
</template>

<style scoped>

.rectangle {
  display: flex;
  text-align: center;
}


span {
  color: rgb(255, 255, 255);
}

</style>