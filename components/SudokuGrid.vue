<script lang="ts" setup>
import { ref } from "vue";

const { puzzle, solution, selectedIndex } = defineProps<{
  puzzle: number[];
  solution: number[];
  selectedIndex: number | null;
}>();

const emit = defineEmits<{
  select: [index: number];
}>();

const selectCell = (index) => {
  emit("select", index);
};

const isSelected = (index) => {
  return selectedIndex !== null && selectedIndex === index;
};

const isOnSameRow = (index) => {
  return (
    selectedIndex !== null &&
    Math.floor(selectedIndex / 9) === Math.floor(index / 9)
  );
};

const isOnSameCol = (index) => {
  return selectedIndex !== null && selectedIndex % 9 === index % 9;
};
</script>

<template>
  <div class="inline-grid grid-cols-9">
    <div
      v-for="(value, i) in puzzle"
      :key="i"
      class="h-10 w-10 flex items-center justify-center font-medium text-gray-800"
      :class="{
        'border-r': (i + 1) % 9 !== 0,
        'border-b': i + 1 < 73,
        'border-r-2 border-gray-300': (i + 1) % 3 === 0 && (i + 1) % 9 !== 0,
        'border-b-2 border-gray-300': i < 72 && i % 27 >= 18,
      }"
    >
      <button
        class="w-full h-full text-sm hover:bg-gray-100 focus:outline-none dark:text-white"
        :class="{
          'bg-violet-100 dark:bg-purple-950': isSelected(i),
          'bg-purple-50 dark:bg-purple-900':
            !isSelected(i) && (isOnSameRow(i) || isOnSameCol(i)),
        }"
        @click="selectCell(i)"
      >
        {{ !!value ? value : null }}
      </button>
    </div>
  </div>
</template>
