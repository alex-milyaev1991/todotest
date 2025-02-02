<script setup>

import {ref, watch} from "vue";

let todos = ref([
    {
        id: 1,
        name: 'first todo',
        isReady: false
    },
]);

let isActiveForm = ref(false);

let newTodo = ref({
    name: "",
    isReady: false
});

watch(
  () => todos,
  (newValue) => {
    console.log("todos", newValue);
    
  },
  { immediate: true }
)

const toogleForm = () => {
    isActiveForm.value = !isActiveForm.value
}
const addTodo = () => {
    newTodo.value.id = Math.floor(Math.random() * 100);
    todos.value.push({...newTodo.value});
    newTodo.value.name = "";
    newTodo.value.isReady = false;
    isActiveForm.value = false;
}

const deleteTodo = (todoId) => {
    const index = todos.value.findIndex(({id}) => id === todoId);
    todos.value.splice(index, 1);
}

</script>

<template>
    <div class="container">
        <h1>Todo list</h1>

        <ul class="todolist">
            <li 
                v-for="item in todos"
                :key="item.id"
                class="todolist-item"
            >
                <span class="todolist-item__name">{{ item.name }}</span>
                <button class="button-delete" @click="deleteTodo(item.id)">X</button>
                <input type="checkbox" :checked="item.isReady">
            </li>
        </ul>
        <form
            v-if="isActiveForm" 
            @submit.prevent="addTodo"
        >
            <div class="row">
                <label for="todoname">Todo name</label>
                <input v-model="newTodo.name" id="todoname" type="text">
            </div>
            <div class="row">
                <label for="todostate">Todo state</label>
                <input v-model="newTodo.isReady" id="todostate" type="checkbox">
            </div>
            <button type="submit">Add todo</button>
        </form>
        <button v-if="!isActiveForm" @click="toogleForm">Add</button>
    </div>
</template>

<style lang="scss">
ul {
    padding: 0;
    margin: 0;
    list-style: none;
}

.container {
    max-width: 320px;
    margin: 0 auto;
}

.row {
    display: flex;
    align-items: center;
    flex-wrap: wrap;
    margin-bottom: 10px;
    input[type="text"] {
        display: block;
        border: 1px solid #ccc;
        padding: 10px;
        width: 100%;
    }
    input[type="checkbox"] {
        margin-left: 10px;
    }
}

.todolist {
    &-item {
        border: 1px solid #ccc;
        display: flex;
        justify-content: space-between;
        gap: 10px;
        padding: 10px;
        width: 100%;
        margin-bottom: 10px;

        .button-delete {
            display: none;
        }
        &:hover {
            .button-delete  {
                display: block;
            }
        }
    }
}
</style>