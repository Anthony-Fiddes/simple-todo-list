<script setup lang="ts">
import { ref, watch } from 'vue'
import type { Ref } from 'vue'
import TodoItem from './components/TodoItem.vue'
import type { TodoState } from './components/TodoItem.vue'

const STORAGE_KEY = "simple-todo-list-store"
const localStoreStr = localStorage.getItem(STORAGE_KEY)
const uncheckedTodos: Ref<TodoState[]> = ref([])
const checkedTodos: Ref<TodoState[]> = ref([])
let todoCounter = ref(0)
if (localStoreStr !== null) {
  const localStore = JSON.parse(localStoreStr)
  const checked = localStore["checked"]
  // could be worth doing more validation to make sure that the arrays are
  // actually properly populated with TodoState objects. But since we're the
  // only ones writing/reading from this store, it's fine for now.
  if (checked && Array.isArray(checked)) {
    checkedTodos.value.push(...localStore["checked"])
  }
  const unchecked = localStore["unchecked"]
  if (unchecked && Array.isArray(unchecked)) {
    uncheckedTodos.value.push(...localStore["unchecked"])
  }
  const checkedIDs = checkedTodos.value.map(x => x.id)
  const uncheckedIDs = uncheckedTodos.value.map(x => x.id)
  const maxID = Math.max(...checkedIDs, ...uncheckedIDs)
  todoCounter.value = maxID + 1
}

function updateLocalStore() {
  localStorage.setItem(STORAGE_KEY, JSON.stringify({ checked: checkedTodos.value, unchecked: uncheckedTodos.value }))
}

watch(uncheckedTodos, updateLocalStore, { deep: true })
watch(checkedTodos, updateLocalStore, { deep: true })

const newTodoTitle = defineModel('newTodo', { type: String, default: "" })

function addTodo() {
  const todo = { title: newTodoTitle.value, checked: false, id: todoCounter.value++ }
  uncheckedTodos.value.unshift(todo)
  newTodoTitle.value = ""
}

function removeTodo(todo: TodoState) {
  const filter = (t: TodoState) => t.id != todo.id
  if (todo.checked) {
    checkedTodos.value = checkedTodos.value.filter(filter)
    return
  }
  uncheckedTodos.value = uncheckedTodos.value.filter(filter)
}

function toggleTodo(todo: TodoState) {
  removeTodo(todo)
  todo.checked = !todo.checked
  if (todo.checked) {
    checkedTodos.value.unshift(todo)
    return
  }
  uncheckedTodos.value.push(todo)
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
        <TodoItem v-for="todo in uncheckedTodos" :key="todo.id" :todo @toggled="toggleTodo(todo)"
          @removed="removeTodo(todo)" />
        <TodoItem v-for="todo in checkedTodos" :key="todo.id" :todo @toggled="toggleTodo(todo)"
          @removed="removeTodo(todo)" />
      </div>
    </div>
  </main>
</template>

<style scoped></style>
