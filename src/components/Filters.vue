<script setup>
import { ref } from 'vue';

const props = defineProps({
    filters: {
        type: Array,
        required: true
    },
    selectedFilter: {
        type: String,
        required: true
    }
});

const chosenFilter = ref(props.filters[0]);
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
            <button v-for="filter in filters" :key="filter" @click="onChange(filter)">
                {{ filter }}
            </button>
        </div>
        <p>The now filter is: {{ selectedFilter }}</p>
    </div>
</template>

