<script setup lang="ts">

import { X } from 'lucide-vue-next';

export type TodoState = {
  title: string,
  checked: boolean,
  id: number
}

const emit = defineEmits(['toggled'])

const props = defineProps<{
  todo: TodoState,
}>()

const cssID = 'task-' + props.todo.id

</script>

<template>
  <div class="field">
    <label class="checkbox" :for="cssID">
      <input :id="cssID" @change="emit('toggled')" :checked="todo.checked" type="checkbox">
      {{ todo.title }}
    </label>
  </div>
</template>

<style scoped>

input {
  margin: 0.4rem;
}

</style>
