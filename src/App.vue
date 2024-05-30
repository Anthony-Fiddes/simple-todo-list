<script setup lang="ts">
import { ref } from 'vue'
import type { Ref } from 'vue'
import TodoItem from './components/TodoItem.vue'
import type { TodoState } from './components/TodoItem.vue'


const newTodoTitle = defineModel('newTodo', { type: String, default: "" })
let todoCounter = ref(0)
const todos: Ref<TodoState[]> = ref([])

function addTodo() {
  todos.value.push({ title: newTodoTitle.value, checked: false, id: todoCounter.value++ })
  newTodoTitle.value = ""
}

</script>

<template>
  <main>
    <input v-model="newTodoTitle" placeholder="New Todo">
    <button @click="addTodo">Add</button>
    <br>
    <br>
    <TodoItem v-for="todo in todos" :key="todo.id" :todo @toggled="todo.checked = !todo.checked" />
  </main>
</template>

<style scoped></style>
