<script setup>
  import {ref, defineProps, defineEmits, onMounted, onBeforeUnmount} from 'vue'

  const props = defineProps({
    toDos: {
      type: Array,
    },
    toDo: {
      type: Object,
    },
    isEditing: {
      type: Object,
    },
  })

  const emits = defineEmits(['update:toDos', 'update:isEditing'])

  const editText = ref('')
  const componentRef = ref()

  const showEdit = () =>
    emits('update:isEditing', {...props.isEditing, [props.toDo.id]: true})
  const hiddenEdit = () =>
    emits('update:isEditing', {...props.isEditing, [props.toDo.id]: false})

  const editToDo = () => {
    editText.value = props.toDo.value
    showEdit()
  }

  const updateToDo = () => {
    if (editText.value.trim() && editText.value !== props.toDo.id) {
      emits(
        'update:toDos',
        props.toDos.map(item =>
          item.id === props.toDo.id ? {...item, value: editText.value} : item,
        ),
      )
    }
    hiddenEdit()
  }

  const deleteToDo = () =>
    emits(
      'update:toDos',
      props.toDos.filter(item => item.id !== props.toDo.id),
    )

  const toggleCheckToDo = () =>
    emits(
      'update:toDos',
      props.toDos.map(item =>
        item.id === props.toDo.id
          ? {...item, isChecked: !item.isChecked}
          : item,
      ),
    )

  const useDetectOutsideClick = component => {
    const handleOutsideClick = event => {
      const isTargetClick =
        event.target !== component.value &&
        !event.composedPath().includes(component.value)
      if (!props.isEditing[props.toDo.id]) return
      if (!isTargetClick) return
      if (!component.value) return
      return hiddenEdit()
    }
    onMounted(() => {
      window.addEventListener('click', handleOutsideClick)
    })
    onBeforeUnmount(() => {
      window.removeEventListener('click', handleOutsideClick)
    })

    return {handleOutsideClick}
  }

  useDetectOutsideClick(componentRef)
</script>

<template>
  <div ref="componentRef" class="todo-list-wrapper">
    <button v-if="toDo.isChecked" @click="toggleCheckToDo">
      <FaIcon icon="circle-check" />
    </button>
    <button v-else @click="toggleCheckToDo">
      <FaIcon icon="fa-regular fa-circle" />
    </button>
    <div>
      <div v-if="isEditing[toDo.id]" class="todo-list">
        <p>
          <input
            v-model="editText"
            :placeholder="toDo"
            @keypress.enter="updateToDo"
          />
        </p>
        <div>
          <button @click="updateToDo">
            <FaIcon icon="check" class="checkbox" />
          </button>
          <button @click="hiddenEdit">
            <FaIcon icon="xmark" />
          </button>
        </div>
      </div>

      <div v-else class="todo-list">
        <p :class="{line: toDo.isChecked}">
          {{ toDo.value }}
        </p>
        <div>
          <button @click="editToDo">
            <FaIcon icon="pen" />
          </button>
          <button @click="deleteToDo">
            <FaIcon icon="trash" />
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<style lang="scss">
  .todo-list-wrapper {
    display: flex;
    margin: 0 20px 10px 20px;
    align-items: center;
    .fa-circle,
    .fa-circle-check {
      margin-top: 2px;
      height: 26px;
      color: #333;
    }
    .todo-list {
      display: flex;
      justify-content: space-between;
      align-items: center;
      font-size: 16px;
      height: 36px;
      margin-left: 10px;
      p {
        width: 280px;
      }
      .line {
        text-decoration: line-through;
      }
      input {
        width: 100%;
        vertical-align: middle;
        padding: 10px 14px;
        border: 1px solid #ddd;
        border-radius: 20px;
        &[placeholder] {
          color: #615f6d;
        }
      }
      div {
        display: flex;
      }
    }
    button {
      background-color: #fff;
      border: none;
      margin-left: 6px;
      cursor: pointer;
    }
  }
</style>
