<script setup lang="ts">
import { ref } from 'vue'
import type { Ref } from 'vue'
import TodoItem from './components/TodoItem.vue'
import type { TodoState } from './components/TodoItem.vue'


const newTodoTitle = defineModel('newTodo', { type: String, default: "" })
let todoCounter = ref(0)
const todos: Ref<TodoState[]> = ref([])

function addTodo() {
  const todo = { title: newTodoTitle.value, checked: false, id: todoCounter.value++ }
  todos.value = [todo, ...todos.value]
  newTodoTitle.value = ""
}

function removeTodo(todo: TodoState) {
  todos.value = todos.value.filter((t) => t.id != todo.id)
}

</script>

<template>
  <main>
    <div class="columns is-centered">
      <div class="column is-half is-size-5">
        <!-- pressing enter will submit the new todo but not reload the page -->
        <form action="javascript:void(0);">
          <div class="field has-addons">
            <div class="control is-expanded">
              <input class="input is-medium" type="text" v-model="newTodoTitle" placeholder="New Todo">
            </div>
            <div class="control">
              <button class="button is-medium" @click="addTodo">Add</button>
            </div>
          </div>
        </form>
        <br>
        <TodoItem v-for="todo in todos" :key="todo.id" :todo @toggled="todo.checked = !todo.checked"
          @removed="removeTodo(todo)" />
      </div>
    </div>
  </main>
</template>

<style scoped></style>
