<script setup lang="ts">

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
  <input :id="cssID" @change="emit('toggled')" :checked="todo.checked" type="checkbox">
  <label :for="cssID">{{ todo.title }}</label>
  <br>
</template>

<style scoped>
input {
  margin: 0.4rem;
}
</style>
