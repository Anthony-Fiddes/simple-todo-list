<script setup lang="ts">

export type TodoState = { title: string, checked: boolean, id: number }

const emit = defineEmits(['toggled'])

defineProps<{
  todo: TodoState,
}>()

</script>

<template>
  <input @change="emit('toggled')" :checked="todo.checked" type="checkbox">
  <label>{{ todo.title }}</label>
  <br>
</template>

<style scoped>
input {
  margin: 0.4rem;
}
</style>
