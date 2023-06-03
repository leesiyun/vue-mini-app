<script setup>
  import {defineProps, defineEmits} from 'vue'

  const props = defineProps({
    modelValue: {
      type: String,
    },
  })

  const emits = defineEmits(['update:modelValue'])

  const changeFilterState = filterState => {
    emits('update:modelValue', filterState)
  }

  const filterStates = ['all', 'active', 'completed']
</script>

<template>
  <div class="filter">
    <button
      v-for="(filterState, index) in filterStates"
      :key="index"
      :class="{active: props.modelValue === filterState}"
      @click="changeFilterState(filterState)"
    >
      ● {{ filterState }}
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
