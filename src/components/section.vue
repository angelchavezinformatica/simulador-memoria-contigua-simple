<script setup lang="ts">
interface Props {
  title: string;
}
defineProps<Props>();
</script>

<template>
  <div class="flex w-full gap-12">
    <div class="flex flex-col gap-2">
      <h3>{{ title }}</h3>

      <slot name="combobox"></slot>
    </div>
    <div class="flex flex-col justify-end gap-2">
      <slot />
    </div>
  </div>
</template>

<style scoped></style>
