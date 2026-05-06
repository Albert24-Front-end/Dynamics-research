<script setup lang="ts" generic="Cols extends string">
const props = defineProps<{
    data: Record<Cols, unknown>[],
    columns: Record<Cols, {title: string}>
}>()
</script>

<template>
    <table>
        <thead>
            <tr>
                <th v-for="col in (Object.keys(columns) as Cols[])" :key="col">
                    {{ columns[col].title}}
                </th>
            </tr>
        </thead>
        <tbody>
            <tr v-for="(item, index) in data" :key="index">
                <td v-for="col in (Object.keys(columns) as Cols[])" :key="col">
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