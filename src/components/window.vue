<script setup lang="ts">
interface Props {
  title?: string;
}
defineProps<Props>();

const windowIcons = [
  { name: "minimize", icon: "minus" },
  { name: "maximize", icon: "window-maximize" },
  { name: "close", icon: "times" },
];
</script>

<template>
  <div class="border-2 border-gray-300 rounded-lg shadow-md">
    <div class="flex p-2 border-b-2 border-gray-300">
      <div class="flex flex-1 gap-4">
        <img src="/favicon.ico" alt="Favicon" width="24" height="24" />
        <h1>{{ title }}</h1>
      </div>

      <div class="flex gap-2">
        <div
          v-for="icon in windowIcons"
          class="hover:bg-gray-700 px-1 rounded-md"
          :key="icon.name"
        >
          <i :class="`pi pi-${icon.icon}`" style="color: white"></i>
        </div>
      </div>
    </div>

    <slot></slot>
  </div>
</template>
