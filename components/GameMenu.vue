<script lang="ts" setup>
import { ref } from "vue";

const emit = defineEmits(["restart"] as const);
</script>

<template>
  <div class="flex flex-col items-center gap-4">
    <h2>Goed gedaan! Ik hou van jou ❤️</h2>

    <button
      class="px-4 py-2 rounded border hover:bg-gray-100"
      @click="emit('restart')"
    >
      Restart
    </button>
  </div>
</template>
