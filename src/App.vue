<script setup lang="ts">
import { computed, ref, watch } from 'vue';
import TimeTable from './components/TimeTable.vue';
import DashboardGitlab from './components/DashboardGitlab.vue';

const mode = ref('lte');
const limit = ref(15);
const value = ref(20);

const limitField = computed(() => mode.value === 'lte' ? 'max' : 'min');

const data = ref([
  {from: 'Samarkand', to: 'Tashkent', price: 17500.50},
  {from: 'Bukhara', to: 'Nukus', price: 14500.70},
  {from: 'Tashkent', to: 'Nukus', price: 30000},
  {from: 'Ferghana', to: 'Tashkent', price: 12000},
  {from: 'Tashkent', to: 'Karshi', price: 15500.30},
]);

const columns = {
  from: {
    title: 'Откуда'
  },
  to: {
    title: 'Куда'
  },
  price: {
    title: 'Цена'
  }
} as const;

watch(limit, (newVal) => {
  value.value = newVal;
})

const dashboardConfig = [
  {
    "id": "filters",
    // "title": "Dashboard panel",
    "gridAttributes": {
      "width": 6,
      "height": 1,
      "yPos": 0,
      "xPos": 3
    }
  },
  {
    "id": "info",
    // "title": "Another dashboard panel",
    "gridAttributes": {
      "width": 3,
      "height": 2,
      "yPos": 1,
      "xPos": 1
    }
  },
  {
    "id": "more",
    // "title": "I can be placed anywhere on the grid",
    "gridAttributes": {
      "width": 4,
      "height": 1,
      "yPos": 2,
      "xPos": 7
    }
  },
] as const;
</script>

<template>
  <div>
    <h1>Hello Vue 3</h1>
    <select v-model="mode">
      <option value="lte">меньше чем или равно</option>
      <option value="gte">больше чем или равно</option>
    </select>
    <input type="number" v-model="limit">
    <br>
    <hr>
    <p>Целевое значение:
      <input type="number" :[limitField]="limit" v-model="value">
    </p>
    <hr>
    <TimeTable :data="data" :columns="columns">
      <template #td(price)="{value}">{{ Intl.NumberFormat("uz-UZ", {style: "currency", currency: "UZS"}).format(Number(value)) }}</template>
    </TimeTable>
    <hr>
    <DashboardGitlab :config="dashboardConfig">
      <template #panel(filters)>My custom stuff</template>
    </DashboardGitlab>
  </div>
</template>
