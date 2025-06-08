<script setup>
import { ref } from 'vue';
import Filters from './Filters.vue';
const users_list_og = [
    { id: 1, name: 'John Doe', department: 'IT' },
    { id: 2, name: 'Jane Smith', department: 'IT' },
    { id: 3, name: 'Jim Beam', department: 'Sales' },
    { id: 3, name: 'Tushar C', department: 'HR' },
]
const users = ref(users_list_og);

const selectedDepartment = ref('All');

function updateFilter(filter) {
    selectedDepartment.value = filter.name;
    console.log(filter.name);
    console.log('Updating list');
    updateList(filter);
}

function updateList(filter) {
    users.value = users_list_og.filter(user => ( filter.name === 'All' ? true : user.department === filter.name));
}
</script>

<template>
    <div>
        <h1>Users List</h1>
        <Filters @update-filter="updateFilter" />
        <p>The current filter is: {{ selectedDepartment }}</p>
        <div class="users-list">
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
</style>