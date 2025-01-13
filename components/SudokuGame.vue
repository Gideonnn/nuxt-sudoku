<script lang="ts" setup>
import { ref, onMounted } from "vue";

const sudoku = ref(null);
const puzzle = ref<number[]>([]);
const solution = ref<number[]>([]);
const selectedIndex = ref<number | null>(null);

const handleSubmit = (value: number) => {
  if (solution.value[selectedIndex.value!] === value) {
    puzzle.value[selectedIndex.value!] = value;
    selectedIndex.value = null;

    window.localStorage.setItem(
      "sudoku",
      JSON.stringify({
        ...sudoku.value,
        puzzle: puzzle.value.join(""),
      })
    );
  }
};

onMounted(() => {
  const sudokuData = window.localStorage.getItem("sudoku");

  if (sudokuData) {
    sudoku.value = JSON.parse(sudokuData);
  } else {
    sudoku.value = getSudoku("medium");
    window.localStorage.setItem("sudoku", JSON.stringify(sudoku.value));
  }

  if (sudoku.value) {
    puzzle.value = sudoku.value.puzzle
      .split("")
      .map((str) => (str === "-" ? 0 : +str));
    solution.value = sudoku.value.solution.split("").map((str) => +str);
  }
});
</script>

<template>
  <SudokuGrid
    :puzzle="puzzle"
    :solution="solution"
    :selectedIndex="selectedIndex"
    @select="(val) => (selectedIndex = val)"
  />

  <InputControl
    class="mt-24 sm:mt-12"
    :puzzle="puzzle"
    :solution="solution"
    :selectedIndex="selectedIndex"
    :disabled="selectedIndex === null || puzzle[selectedIndex] !== 0"
    @click="handleSubmit"
  />
</template>
