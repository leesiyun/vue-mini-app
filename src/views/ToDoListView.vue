<script setup>
  import ToDoItem from '../components/ToDoItem.vue'
  import {ref, watch} from 'vue'

  const localStorageItem = localStorage.getItem('toDos')
  const toDos = ref(localStorageItem ? JSON.parse(localStorageItem) : [])
  const filteredToDos = ref(toDos.value ? toDos.value : [])
  const filterState = ref('all')
  const isEditing = ref({})
  const newToDo = ref('')

  const saveLocalStorage = () =>
    localStorage.setItem('toDos', JSON.stringify(toDos.value))

  const addToDo = () => {
    if (newToDo.value.trim()) {
      toDos.value.push({
        id: toDos.value.length > 0 ? toDos.value.at(-1).id + 1 : 1,
        value: newToDo.value,
        isChecked: false,
      })
      saveLocalStorage()
    }
    newToDo.value = ''
  }

  const updateToDo = (id, value) => {
    toDos.value = toDos.value.map(item =>
      item.id === id ? {...item, value: value} : item,
    )
    saveLocalStorage()
  }

  const deleteToDo = id => {
    toDos.value = toDos.value.filter(item => item.id !== id)
    saveLocalStorage()
  }

  const toggleCheckToDo = id => {
    toDos.value = toDos.value.map(item =>
      item.id === id ? {...item, isChecked: !item.isChecked} : item,
    )
    saveLocalStorage()
  }

  const resetToDos = () => {
    toDos.value = []
    localStorage.removeItem('toDos')
  }

  const changeEdit = (id, boolean) => (isEditing.value[id] = boolean)

  const getFilteredToDos = state => {
    if (state === 'all') filteredToDos.value = toDos.value
    if (state === 'active')
      filteredToDos.value = toDos.value.filter(item => !item.isChecked)
    if (state === 'completed')
      filteredToDos.value = toDos.value.filter(item => item.isChecked)
  }

  const changeFilterState = state => {
    filterState.value = state
    getFilteredToDos(filterState.value)
  }

  watch(toDos, () => getFilteredToDos(filterState.value))
</script>

<template>
  <div class="todo">
    <div class="todo-wrapper">
      <div class="todo-input">
        <input v-model="newToDo" @keypress.enter="addToDo" />
        <div>
          <button @click="addToDo">
            <FaIcon icon="plus" />
          </button>
          <button @click="resetToDos">
            <FaIcon icon="arrow-rotate-left" />
          </button>
        </div>
      </div>
      <div class="filter">
        <button @click="changeFilterState('all')">● all</button>
        <button @click="changeFilterState('active')">● active</button>
        <button @click="changeFilterState('completed')">● completed</button>
      </div>
      <ToDoItem
        v-for="toDo in filteredToDos"
        :key="toDo.id"
        :to-do="toDo"
        :is-editing="isEditing"
        @update-to-do="updateToDo"
        @delete-to-do="deleteToDo"
        @toggle-check-to-do="toggleCheckToDo"
        @change-edit="changeEdit"
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
      .todo-input {
        display: flex;
        justify-content: space-between;
        width: 386px;
        margin: 30px auto 20px auto;
        input {
          width: 280px;
          height: 40px;
          border-radius: 20px;
          border: 1px solid #ddd;
          padding: 0 20px;
          font-size: 16px;
        }
        button {
          width: 40px;
          height: 40px;
          border-radius: 50%;
          border: 1px solid #ddd;
          font-size: 20px;
          margin-left: 8px;
        }
      }
      .filter {
        margin: 0px 20px 20px 30px;
        button {
          margin-right: 10px;
          padding: 6px 10px;
          border: 1px solid #ddd;
          border-radius: 20px;
        }
      }
    }
  }
</style>
