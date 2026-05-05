<script setup>
import { computed } from 'vue';

const props = defineProps({
    config: {
        type: Array,
        required: true
    }
})

// const gridAreas = computed(() => [`"${props.config[0].id} ${props.config[0].id} ${props.config[0].id}"`,
//         `"${props.config[1].id} . ."`,
//         `"${props.config[1].id} ${props.config[2].id} ${props.config[2].id}"`
// ].join('\n'));

const panelStyles = computed(() => {
    return Object.fromEntries(props.config.map((entry) => [entry.id, {
        gridColumn: `${entry.gridAttributes.xPos + 1} / ${entry.gridAttributes.xPos + entry.gridAttributes.width + 1}`,
        gridRow: `${entry.gridAttributes.yPos + 1} / ${entry.gridAttributes.yPos + entry.gridAttributes.height + 1}`,
    }]))
    // + 1 добавляется, так как в CSS Grid линии нумеруются (индексация начинается) с 1, а у нас координаты задаются с нуля
})

</script>

<template>
    <div class="grid">
        <div v-for="entry in config" :key="entry.id" class="panel" :style="panelStyles[entry.id]">
        <!-- <div v-for="entry in config" :key="entry.id" class="panel" :style="{ gridArea: entry.id }"> -->
            <slot :name="`panel(${entry.id})`">{{ entry.id }}</slot>
        </div>
    </div>
</template>

<style scoped>
.panel {
    border: 1px solid red;
    margin: 5px;
    min-height: 10px;
}

.grid {
    display: grid;
    grid-template-columns: repeat(12, 1fr);
    grid-template-rows: repeat(3, minmax(40px, auto));
  /*  grid-template-areas: v-bind(gridAreas); */

    grid-gap: 8px;
    width: 100%;
}
</style>