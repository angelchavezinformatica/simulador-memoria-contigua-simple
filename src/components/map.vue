<script setup lang="ts">
import { computed } from "vue";

interface Props {
  memorySize: number;
  operatingSystem: { name: string; size: number };
  process?: { name: string; size: number };
}

const props = defineProps<Props>();

const usedMemory = computed(() => {
  return props.operatingSystem.size + (props.process?.size ?? 0);
});

const freeMemory = computed(() => {
  return props.memorySize - usedMemory.value;
});
</script>

<template>
  <div class="memory-container">
    <!-- Mapa de memoria -->
    <div class="memory-map" :style="{ height: props.memorySize + 'px' }">
      <!-- SO -->
      <div
        class="block os"
        :style="{ height: props.operatingSystem.size + 'px' }"
      >
        <span class="label">{{ props.operatingSystem.name }}</span>
      </div>

      <!-- Proceso -->
      <div
        v-if="props.process"
        class="block process"
        :style="{ height: props.process.size + 'px' }"
      >
        <span class="label">{{ props.process.name }}</span>
      </div>

      <!-- Memoria libre -->
      <div
        v-if="freeMemory > 0"
        class="block free"
        :style="{ height: freeMemory + 'px' }"
      >
        <span class="label">Memoria disponible no usable</span>
      </div>
    </div>

    <!-- Columnas de tamaños -->
    <div class="memory-sizes" :style="{ height: props.memorySize + 'px' }">
      <div
        class="size-label"
        :style="{ height: props.operatingSystem.size + 'px' }"
      >
        {{ props.operatingSystem.size }} KB
      </div>
      <div
        v-if="props.process"
        class="size-label"
        :style="{ height: props.process.size + 'px' }"
      >
        {{ props.process.size }} KB
      </div>
      <div
        v-if="freeMemory > 0"
        class="size-label"
        :style="{ height: freeMemory + 'px' }"
      >
        {{ freeMemory }} KB
      </div>
    </div>
  </div>
</template>

<style scoped>
.memory-container {
  display: flex;
  gap: 0.2rem;
  align-items: flex-start;
  font-family: sans-serif;
  background-color: #101828;
  padding: 1rem;
  color: #f1f5f9;
}

.memory-map {
  width: 180px;
  border: 2px solid #475569;
  display: flex;
  flex-direction: column;
}

.memory-sizes {
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  font-size: 0.85rem;
  color: #cbd5e1;
}

.block {
  position: relative;
  width: 100%;
  border-bottom: 1px solid #334155;
  display: flex;
  justify-content: center;
  align-items: center;
  overflow: hidden;
  color: #fff;
}

.os {
  background-color: #2e8b57;
}

.process {
  background-color: #1e90ff;
}

.free {
  background-color: #6b7280;
}

.label {
  font-weight: bold;
  text-align: center;
  padding: 0.2rem;
}

.size-label {
  display: flex;
  align-items: center;
  padding-left: 0.5rem;
  border-bottom: 1px solid #334155;
}
</style>
