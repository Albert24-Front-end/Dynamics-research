<script setup>
import { computed } from 'vue';

const props = defineProps({
    config: {
        type: Array,
        required: true
    }
})

// const panelStyles = computed(() => {
//     return Object.fromEntries(props.config.map((entry) => [entry.id, {
//         gridColumn: `${entry.gridAttributes.xPos + 1} / ${entry.gridAttributes.xPos + entry.gridAttributes.width + 1}`,
//         gridRow: `${entry.gridAttributes.yPos + 1} / ${entry.gridAttributes.yPos + entry.gridAttributes.height + 1}`,
//     }]))
//     // + 1 добавляется, так как в CSS Grid линии нумеруются (индексация начинается) с 1, а у нас координаты задаются с нуля
// })

const columns = 12;
const rows = computed(() =>
  Math.max( ...props.config.map(({gridAttributes}) => gridAttributes.yPos + gridAttributes.height) )
)

const gridAreas = computed(() => {
    const grid = Array.from({length: rows.value}, (_, i) => Array.from({length: columns}).fill('.'));

    props.config.map((entry) => {
        const { id, gridAttributes } = entry;
        const { xPos, yPos, width, height } = gridAttributes;

        for (let i = 0; i < width; i++) {
            for (let j = 0; j < height; j++) {
                grid[yPos + j][xPos + i] = id;
            }
        }
    })

    const result = grid.map((row) => `"${row.join(' ')}"`).join('\n');
    return result;
});

</script>

<template>
    <div class="grid">
        <!-- <div v-for="entry in config" :key="entry.id" class="panel" :style="panelStyles[entry.id]"> -->
        <div v-for="entry in config" :key="entry.id" class="panel" :style="{ gridArea: entry.id }">
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
    grid-template-areas: v-bind(gridAreas);

    grid-gap: 8px;
    width: 100%;
}
</style>