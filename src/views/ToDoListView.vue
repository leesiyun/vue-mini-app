<script setup>
  import {ToDoInput, ToDoFilter, ToDoItem} from '@/components/toDoList'
  import {ref, watch} from 'vue'

  const toDos = ref(JSON.parse(localStorage.getItem('toDos')) || [])
  const filterState = ref('all')
  const isEditing = ref({})

  const saveLocalStorage = () =>
    localStorage.setItem('toDos', JSON.stringify(toDos.value))

  const getFilteredToDos = filterState => {
    if (filterState === 'all') return toDos.value
    if (filterState === 'active')
      return toDos.value.filter(({isChecked}) => !isChecked)
    if (filterState === 'completed')
      return toDos.value.filter(({isChecked}) => isChecked)
  }

  watch(toDos, () => {
    saveLocalStorage()
    getFilteredToDos(filterState.value)
  })
</script>

<template>
  <div class="todo">
    <div class="todo-wrapper">
      <ToDoInput v-model="toDos" />
      <ToDoFilter v-model="filterState" />
      <ToDoItem
        v-for="toDo in getFilteredToDos(filterState)"
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
