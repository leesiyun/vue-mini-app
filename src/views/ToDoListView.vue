<script setup>
  import {ToDoInput, ToDoFilter, ToDoItem} from '@/components/toDoList'
  import {ref, watch} from 'vue'

  const localStorageItem = localStorage.getItem('toDos')
  const toDos = ref(localStorageItem ? JSON.parse(localStorageItem) : [])
  const filteredToDos = ref(toDos.value ? toDos.value : [])
  const filterState = ref('all')
  const isEditing = ref({})

  const saveLocalStorage = () =>
    localStorage.setItem('toDos', JSON.stringify(toDos.value))

  const getFilteredToDos = state => {
    if (state === 'all') filteredToDos.value = toDos.value
    if (state === 'active')
      filteredToDos.value = toDos.value.filter(item => !item.isChecked)
    if (state === 'completed')
      filteredToDos.value = toDos.value.filter(item => item.isChecked)
  }

  watch(toDos, () => {
    saveLocalStorage()
    getFilteredToDos(filterState.value)
  })
</script>

<template>
  <div class="todo">
    <div class="todo-wrapper">
      <ToDoInput v-model:toDos="toDos" />
      <ToDoFilter
        v-model:filterState="filterState"
        @get-filtered-to-dos="getFilteredToDos"
      />
      <ToDoItem
        v-for="toDo in filteredToDos"
        :key="toDo.id"
        :to-do="toDo"
        v-model:toDos="toDos"
        v-model:isEditing="isEditing"
      />
    </div>
  </div>
</template>

<style lang="scss">
  .todo {
    display: flex;
    justify-content: center;
    align-items: center;
    min-height: 100vh;
    .todo-wrapper {
      display: flex;
      flex-direction: column;
      border: 1px solid #ddd;
      box-shadow: 0 0 12px #dddddd;
      width: 440px;
      height: 500px;
      border-radius: 20px;
    }
  }
</style>
