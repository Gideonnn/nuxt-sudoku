<script lang="ts" setup>
import { ref, onMounted } from "vue";
import { getSudoku } from "sudoku-gen";
</script>

<template>
  <NuxtRouteAnnouncer />

  <div class="px-4 py-8 flex flex-col items-center">
    <ClientOnly>
      <SudokuGame />
    </ClientOnly>
  </div>
</template>
