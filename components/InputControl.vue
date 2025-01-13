<script lang="ts" setup>
import { ref } from "vue";

const options = [1, 2, 3, 4, 5, 6, 7, 8, 9];

const { puzzle, solution, selectedIndex, disabled } = defineProps<{
  puzzle: number[];
  solution: number[];
  selectedIndex: number | null;
  disabled: boolean;
}>();

const emit = defineEmits<{
  click: [index: number];
}>();
</script>

<template>
  <div
    class="grid grid-cols-3 gap-2"
    :class="{
      'pointer-events-none opacity-40': disabled,
    }"
  >
    <button
      v-for="value in options"
      class="w-16 h-16 rounded border hover:bg-gray-100 focus:outline-none"
      :class="{
        'bg-gray-100': puzzle[selectedIndex] === value,
        'bg-yellow-50': !disabled && solution[selectedIndex] === value,
      }"
      @click="emit('click', value)"
    >
      {{ value }}
    </button>
  </div>
</template>
