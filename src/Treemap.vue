<script setup lang="ts">
import * as d3 from 'd3';
import { onMounted } from 'vue';

export interface DataInterface {
  name: string,
  value: number
}

const props = defineProps<{
    data: { name: string, value: number }[],
    title: string
}>()

function treemap(data: DataInterface[]): void {
  // organiza o array
  data.sort((a, b) => b.value - a.value)
  //soma os valores do array
  const sum = data.reduce((s, i) => s + i.value, 0);
  // declara o retangulo
  const svg = d3.select('svg');
  const width = parseInt(svg.attr('width')!);
  const height = parseInt(svg.attr('height')!);
  // salva as coordenadas do espaço vazio
  const bounds = { startTop: 0, startLeft: 0, right: width, bottom: height };

  // quanto falta dos valores 
  let weightLeft = sum;
  // declar os eixos e tamanhos do retangulo a ser gerado
  let eixoX, eixoY, rectangleWidth, rectangleHeigth;

  // declara a cor inical retangulo
  let color1 = 81;
  let color2 = 81;
  let color3 = 250;

  // itera pelo array de numeros e gera o retangulo
  data.forEach(el => {
    const horizontalVoidSpace = bounds.right - bounds.startLeft;
    const verticalVoidSpace = bounds.bottom - bounds.startTop;
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
    svg.append('rect')
      .attr('x', eixoX)
      .attr('y', eixoY)
      .attr('width', rectangleWidth)
      .attr('height', rectangleHeigth)
      .style('fill', `rgb(${color1}, ${color2}, ${color3})`)
      .style('stroke', 'white')
      .style('stroke-width', 3);
    svg.append('text')
      .text(`${el.name} - ${el.value}`)
      .attr('x', eixoX + rectangleWidth / 2)
      .attr('y', eixoY + rectangleHeigth / 2)
      .attr('text-anchor', 'middle')
      .attr('alignment-baseline', 'middle')
      .style('fill', 'white')
      .style('shadow', 'black');

    color1+= 15;
    color2+= 15;
  });
}

onMounted(() => {
  treemap(props.data)
});

</script>

<template>
    <div class="block">
      <div>
        <h2>{{ props.title }}</h2>
        <svg width="400" height="400"></svg>
      </div>
      <div class="side">
        <h4>Lista: </h4>
        <span v-for="item in props.data">
            {{ item.name }} {{ item.value }}
        </span>
      </div>
    </div>
</template>

<style scoped>

.block {
    padding: 10px;
    border: 1px solid black;
    display: flex;
}

.side {
    border: 1px solid black;
    display: flex;
    flex-direction: column;
    padding: 10px;
    padding-right: 50px;
    margin: 5px;
}

span {
    padding-bottom: 5px;
}

</style>