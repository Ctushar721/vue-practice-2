<script setup>
import { ref } from 'vue';

const filters = ref([
    { id: 1, name: 'All' },
    { id: 2, name: 'IT' },
    { id: 3, name: 'HR' },
    { id: 4, name: 'Sales' },
]);
const chosenFilter = ref(filters.value[0]);
const emit = defineEmits(['update-filter']);

function onChange(filter) {
    chosenFilter.value = filter;
    emit('update-filter', filter);
}
</script>

<template>
    <div>
        <h1>Filters</h1>
        <div>
            <button v-for="filter in filters" :key="filter.id" @click="onChange(filter)">
                {{ filter.name }}
            </button>
        </div>
    </div>
</template>

