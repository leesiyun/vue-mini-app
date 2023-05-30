<script setup>
  import {ref, computed, defineEmits} from 'vue'

  const props = defineProps({
    toDos: {
      type: Array,
    },
  })

  const emits = defineEmits(['update:toDos'])

  const newToDo = ref('')

  const addToDo = () => {
    if (newToDo.value.trim()) {
      emits('update:toDos', [
        ...props.toDos,
        {
          id: props.toDos.length > 0 ? props.toDos.at(-1).id + 1 : 1,
          value: newToDo.value,
          isChecked: false,
        },
      ])
    }
    newToDo.value = ''
  }

  const resetToDos = () => {
    emits('update:toDos', [])
  }
</script>

<template>
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
</template>

<style lang="scss">
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
      cursor: pointer;
    }
  }
</style>
