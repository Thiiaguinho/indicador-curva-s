<template>
  <div ref="vegaLiteContainer"></div>
</template>

<script setup lang="ts">
import { ref, onMounted, watch } from 'vue';
import vegaEmbed from 'vega-embed';
import type { VisualizationSpec } from 'vega-embed';

const vegaLiteContainer = ref<HTMLDivElement | null>(null);
const windowWidth = ref(0);

const valoresSemanais = [
  { Semana: 'Semana 01', Previsto: 1.5, Realizado: 1.6 },
  { Semana: 'Semana 02', Previsto: 3.75, Realizado: 3.95 },
  { Semana: 'Semana 03', Previsto: 7.15, Realizado: 7.15 },
  { Semana: 'Semana 04', Previsto: 12.05, Realizado: 11.55 },
  { Semana: 'Semana 05', Previsto: 17.15, Realizado: 16.55 },
  { Semana: 'Semana 06', Previsto: 23.30, Realizado: 21.65 },
  { Semana: 'Semana 07', Previsto: 30.10, Realizado: 26.99 },
  { Semana: 'Semana 08', Previsto: 37.20, Realizado: 32.69 },
  { Semana: 'Semana 09', Previsto: 45.00, Realizado: 38.44 },
  { Semana: 'Semana 10', Previsto: 52.90, Realizado: 44.00 },
  { Semana: 'Semana 11', Previsto: 61.05, Realizado: 50.00 }, 
  { Semana: 'Semana 12', Previsto: 69.35, Realizado: null },
  { Semana: 'Semana 13', Previsto: 76.85, Realizado: null },
  { Semana: 'Semana 14', Previsto: 83.05, Realizado: null },
  { Semana: 'Semana 15', Previsto: 88.10, Realizado: null },
  { Semana: 'Semana 16', Previsto: 92.30, Realizado: null },
  { Semana: 'Semana 17', Previsto: 96.10, Realizado: null },
  { Semana: 'Semana 18', Previsto: 98.45, Realizado: null },
  { Semana: 'Semana 19', Previsto: 100.00, Realizado: null },
];

const data = [
...valoresSemanais.map(item => ({
  Semana: item.Semana,
  Valor:item.Previsto,
  Categoria: 'Previsto',
})),
...valoresSemanais.map(item => ({
  Semana: item.Semana,
  Valor:item.Realizado,
  Categoria: 'Realizado',
}))
];

const dataTransformada = valoresSemanais.flatMap((d) => [
  { ...d, Categoria: 'Previsto', Valor: d.Previsto },
  d.Realizado !== null ? { ...d, Categoria: 'Realizado', Valor: d.Realizado } : null
]).filter(Boolean);

const spec: VisualizationSpec = {
  $schema: 'https://vega.github.io/schema/vega-lite/v5.json',
  description: 'Evolução do Projeto de Software em base semanal',
  data: { values: dataTransformada },
  width: "container",
  height: 400,
  encoding: {
    x: { field: 'Semana', type: 'ordinal', axis: { title: 'Semana' } },
  },
  layer: [
    {
      mark: 'line',
      encoding: {
        y: { field: 'Valor', type: 'quantitative', axis: { title: 'Porcentagem (%)' } },
        color: { field: 'Categoria', type: 'nominal', legend: { title: 'Categoria' } },
      },
    },
    {
      transform: [
        {
          filter: "datum.Categoria == 'Realizado'"
        },
        {
          calculate: "datum.Valor",
          as: "Realizado"
        },
        {
          calculate: "datum.Previsto",
          as: "Previsto"
        }
      ],
      mark: {
        type: 'errorband',
        extent: 'ci'
      },
      encoding: {
        y: {
          field: 'Previsto',
          type: 'quantitative',
        },
        y2: {
          field: 'Realizado',
        },
        color: {
          condition: { test: "datum.Realizado > datum.Previsto", value: 'red' },
          value: 'green'
        }
      }
    }
  ]
};

onMounted(() => {
  windowWidth.value = window.innerWidth;

  const handleResize = () => {
    windowWidth.value = window.innerWidth;
  };

  window.addEventListener('resize', handleResize);

  onUnmounted(() => {
    window.removeEventListener('resize', handleResize);
  });

  if (vegaLiteContainer.value) {
  }
});



watch(windowWidth, () => {
  if (vegaLiteContainer.value) {
    vegaEmbed(vegaLiteContainer.value, {
      ...spec,
      width: windowWidth.value < 500 ? 200 : windowWidth.value > 900 ? 900 - 170 : windowWidth.value - 170,
    }, { actions: false }).catch(console.error);
  }
});
</script>
