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
                <th v-for="col in Object.values(columns)" :key="col.price">
                    {{ col.title }}
                </th>
            </tr>
        </thead>
        <tbody>
            <tr v-for="item in data">
                <td v-for="col in Object.keys(columns)" :key="col.price">
                    <slot :name="`td(${col})`" v-bind="{item, value:item[col]}">{{ item[col] }}</slot>
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