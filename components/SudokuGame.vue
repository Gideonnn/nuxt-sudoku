<script lang="ts" setup>
import { ref, onMounted } from "vue";
import { getSudoku } from "sudoku-gen";
import ConfettiExplosion from "vue-confetti-explosion";

const sudoku = ref(null);
const puzzle = ref<number[]>([]);
const solution = ref<number[]>([]);
const selectedIndex = ref<number | null>(null);
const finished = ref(false);

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

  if (puzzle.value.join("") === solution.value.join("")) {
    finished.value = true;
  }
};

const handleInit = () => {
  const sudokuData = window.localStorage.getItem("sudoku");

  if (sudokuData) {
    sudoku.value = JSON.parse(sudokuData);
  } else {
    sudoku.value = getSudoku("medium");
    window.localStorage.setItem("sudoku", JSON.stringify(sudoku.value));
  }

  if (sudoku.value.puzzle === sudoku.value.solution) {
    finished.value = true;
  }

  if (sudoku.value) {
    puzzle.value = sudoku.value.puzzle
      .split("")
      .map((str) => (str === "-" ? 0 : +str));
    solution.value = sudoku.value.solution.split("").map((str) => +str);
  }
};

const handleRestart = () => {
  window.localStorage.removeItem("sudoku");
  finished.value = false;
  handleInit();
};

onMounted(handleInit);
</script>

<template>
  <ConfettiExplosion
    v-if="finished"
    class="top-[-60px]"
    :particleCount="400"
    :particleSize="15"
    :stageHeight="1000"
    :stageWidth="600"
    :force="0.6"
  />

  <SudokuGrid
    :puzzle="puzzle"
    :solution="solution"
    :selectedIndex="selectedIndex"
    @select="(val) => (selectedIndex = val)"
  />

  <div class="mt-24 sm:mt-12">
    <InputControl
      v-show="!finished"
      :puzzle="puzzle"
      :solution="solution"
      :selectedIndex="selectedIndex"
      :disabled="selectedIndex === null || puzzle[selectedIndex] !== 0"
      @click="handleSubmit"
    />

    <GameMenu v-show="finished" @restart="handleRestart" />
  </div>
</template>
