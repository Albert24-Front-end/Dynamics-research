<script setup lang="ts">
const props = defineProps({
    data: {
        type: Array,
        required: true,
    },
    columns: {
        type: Object,
        req: true,
    }
})
</script>

<template>
    <table>
        <thead>
            <tr>
                <th v-for="col in Object.values(columns)">
                    {{ col.title }}
                </th>
            </tr>
        </thead>
        <tbody>
            <tr v-for="item in data">
                <td v-for="col in Object.keys(columns)">
                    {{ item[col] }}
                </td>
            </tr>
        </tbody>
    </table>
    <div>
        <slot></slot>
    </div>
</template>

<style scoped>
td {
    border: 1px solid black;
}
</style>