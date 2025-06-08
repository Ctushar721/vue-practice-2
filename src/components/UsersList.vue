<script setup>
import { ref, onMounted } from 'vue';
import axios from 'axios';
import Filters from './Filters.vue';

const users_list_og = ref([]);
const users = ref([]);
const isLoading = ref(true);
const error = ref(null);

const filters = ref(['All']); // NOTE: it makes a copy and not a reference

const selectedDepartment = ref('All');

async function fetchUsers() {
    try {
        const { data } = await axios.request({ // NOTE: We need to allow cross origin requests in BE's server.js
            method: 'GET',
            url: 'http://localhost:3000/users'
        });
        users_list_og.value = data;
        users.value = data;
        // Update filters based on fetched departments
        filters.value = ['All', ...new Set(data.map(user => user.department))];
    } catch (err) {
        error.value = err.message;
    } finally {
        isLoading.value = false;
    }
}

function updateFilter(filter) {
    selectedDepartment.value = filter;
    console.log(filter);
    console.log('Updating list');
    updateList(filter);
}

function updateList(filter) {
    users.value = users_list_og.value.filter(user => (filter === 'All' ? true : user.department === filter));
}

onMounted(() => {
    fetchUsers();
});
</script>

<template>
    <div>
        <h1>Users List</h1>
        <Filters :filters="filters" selectedFilter="selectedDepartment" @update-filter="updateFilter" /> <!-- NOTE: selectedFilter is a static prop, not dynamic -->
        <p>The current filter is: {{ selectedDepartment }}</p>
        
        <div v-if="isLoading" class="loading">
            Loading users...
        </div>
        
        <div v-else-if="error" class="error">
            Error: {{ error }}
        </div>
        
        <div v-else class="users-list">
            <div class="header">
                <div class="name">Name</div>
                <div class="department">Department</div>
            </div>
            <ul>
                <li v-for="user in users" :key="user.id">
                    <div class="name">{{ user.name }}</div>
                    <div class="department">{{ user.department }}</div>
                </li>
            </ul>
        </div>
    </div>
</template>

<style scoped>
.users-list {
    max-width: 600px;
    margin: 0 auto;
}

.header {
    display: grid;
    grid-template-columns: 1fr 1fr;
    padding: 10px;
    background-color: #1a37d9;
    font-weight: bold;
    border-bottom: 2px solid #ddd;
}

ul {
    list-style: none;
    padding: 0;
    margin: 0;
}

li {
    display: grid;
    grid-template-columns: 1fr 1fr;
    padding: 10px;
    border-bottom: 1px solid #eee;
}

li:hover {
    background-color: #33d2eb;
}

.loading, .error {
    text-align: center;
    padding: 20px;
    font-size: 1.2em;
}

.error {
    color: red;
}
</style>