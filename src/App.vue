<script setup lang="ts">
import { computed, ref, watch } from 'vue';

const mode = ref('lte');
const limit = ref(15);
const value = ref(20);

const limitField = computed(() => mode.value === 'lte' ? 'max' : 'min')

// const computedVal = computed({
//   get() {
//     return mode.value === 'lte' ? Math.min(value.value, limit.value) : Math.max(value.value, limit.value);
//   },
//   set(newVal) {
//     value.value = newVal;
//   }
// })

watch(limit, (newVal) => {
  value.value = newVal;
})
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
      <!-- <input v-if="mode ==='gte'" type="number" v-model="value" :min="limit"> -->
    </p>
  </div>
</template>
