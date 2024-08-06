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
  border?: string,
  position: string,
  display?: string
  name?: string,
  value?: number
  fontSize?: string;
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

const hoveredIndex = ref<number | null>(null);

function treemap(
  data: DataInterface[], 
  width: number, 
  height: number, 
  color1: number, 
  color2: number, 
  color3: number,
  declareSquer: boolean = true,
  y: number = 0, 
  x: number = 0, 
  ): void {

  if (!data || data.length === 0) return;

  data.sort((a, b) => b.value - a.value)
  const sum: number = data.reduce((s, i) => s + i.value, 0);
  const bounds: boundsInterface = { startTop: y, startLeft: x, right: width, bottom: height };

  let weightLeft: number = sum;
  let eixoX: number, eixoY: number, rectangleWidth: number, rectangleHeigth: number;

  if (declareSquer) {
    treemapData.value.squere = { 
      top: `${bounds.startTop}px`, 
      left: `${bounds.startLeft}px`, 
      height: `${bounds.bottom}px`, 
      width: `${bounds.right}px`,
      backgroundColor: `white`,
      position: 'relative',
    }
  }

  data.forEach(el => {
    const horizontalVoidSpace: number = bounds.right - bounds.startLeft;
    const verticalVoidSpace: number = bounds.bottom - bounds.startTop;
    
    eixoX = bounds.startLeft;
    eixoY = bounds.startTop;

    if (horizontalVoidSpace > verticalVoidSpace) {
      rectangleWidth = (el.value / weightLeft) * horizontalVoidSpace;
      rectangleHeigth = verticalVoidSpace ;
      bounds.startLeft = eixoX + rectangleWidth;

    } else {
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
      value: el.value,
      fontSize: `${(rectangleWidth + rectangleHeigth) * 0.075}px`
    })

    color2+= 20;
    
    if (el.children && el.children.length > 0) {
        treemap(el.children, eixoX + rectangleWidth, eixoY + rectangleHeigth, color1 + 40, color2 + 40, color3, false, eixoY, eixoX)
    }
  });
}

onMounted(() => {
  treemap(props.data, props.width, props.height, 0, 90, 0)
});

</script>

<template>
  <div class="container2">
    <h1>{{ props.title }}</h1>
    <div :style="{...treemapData.squere, boxShadow: '15px 15px 5px rgba(0, 0, 0, 0.5)' }">
      <div 
        v-for="(rectangle, index) in treemapData.rectangles" 
        :key="index"
        :style="{
          ...rectangle, 
          padding: '5px',
          overflow: 'hidden',
          display: 'flex',
          flexDirection: 'column',
          }"
          @mouseover="hoveredIndex = index"
          @mouseleave="hoveredIndex = null"
        >
          <span>{{ rectangle.name }}</span>
          <span v-if="hoveredIndex === index">{{ rectangle.value }}</span>
      </div>
    </div>
  </div>
</template>

<style scoped>

.container2 {
  margin-top: 10xp;
  border: 1px solid black;
  border-radius: 6px;
  padding: 3%;
  background-color:rgb(255, 251, 246);
  box-shadow: 10px 10px 5px rgba(0, 0, 0, 0.5);
  height: auto;
  width: auto;
}

span {
  color: rgb(255, 255, 255);
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.8);
}
</style>