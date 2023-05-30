<script setup>
  import {defineProps, defineEmits} from 'vue'

  const props = defineProps({
    filterState: {
      type: String,
    },
  })

  const emits = defineEmits(['update:filterState', 'getFilteredToDos'])

  const changeFilterState = state => {
    emits('update:filterState', state)
    emits('getFilteredToDos', state)
  }
</script>

<template>
  <div class="filter">
    <button
      :class="{active: props.filterState === 'all'}"
      @click="changeFilterState('all')"
    >
      ● all
    </button>
    <button
      :class="{active: props.filterState === 'active'}"
      @click="changeFilterState('active')"
    >
      ● active
    </button>
    <button
      :class="{active: props.filterState === 'completed'}"
      @click="changeFilterState('completed')"
    >
      ● completed
    </button>
  </div>
</template>

<style lang="scss">
  .filter {
    margin: 0px 20px 20px 30px;
    button {
      margin-right: 10px;
      padding: 6px 10px;
      border: 1px solid #ddd;
      border-radius: 20px;
      cursor: pointer;
    }
    .active {
      border: 2px solid #333;
    }
  }
</style>
