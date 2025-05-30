<script setup lang="ts">
import "primeicons/primeicons.css";
import { computed, ref } from "vue";
import Select from "primevue/select";
import Button from "primevue/button";
import { data } from "./data";
import Map from "./components/map.vue";
import Section from "./components/section.vue";
import Window from "./components/window.vue";
import IconBroom from "./components/icons/broom.vue";

const operatingSystemSelection = ref(data.operatingSystems[0]);
const processSelection = ref();

const internFragmentation = computed(() => {
  return (
    data.memorySize -
    operatingSystemSelection.value.size -
    (processSelection.value ? processSelection.value.size : 0)
  );
});

const clearMemory = () => {
  processSelection.value = null;
};
</script>

<template>
  <main class="flex items-center justify-center w-full h-screen">
    <Window title="Simulador de Memoria Contigua Simple" class="w-[800px]">
      <div class="flex w-full gap-4 px-4 py-4">
        <div class="flex flex-col">
          <h2 class="text-center py-2">
            Tamaño de Memoria: {{ data.memorySize }} KB
          </h2>

          <div class="flex flex-col gap-8 w-full">
            <Section title="Sistema Operativo">
              <template #combobox>
                <div class="flex w-56">
                  <Select
                    v-model="operatingSystemSelection"
                    :options="data.operatingSystems"
                    optionLabel="name"
                    fluid
                  />
                </div>
              </template>

              <p>Toma: {{ operatingSystemSelection.size }} KB</p>
              <p>
                Memoria Disponible:
                {{ data.memorySize - operatingSystemSelection.size }} KB
              </p>
            </Section>

            <Section title="Proceso">
              <template #combobox>
                <div class="flex w-56">
                  <Select
                    v-model="processSelection"
                    :options="data.processes"
                    optionLabel="name"
                    placeholder="Seleccione un proceso"
                    fluid
                  />
                </div>
              </template>

              <p v-if="processSelection">
                Tamaño: {{ processSelection.size }} KB
              </p>
            </Section>
          </div>

          <div class="flex w-full py-6">
            <div class="flex flex-col gap-8 flex-1">
              <h3 class="border-b-1 px-1">Información</h3>

              <p>Fragmentación interna: {{ internFragmentation }} KB</p>
              <p>
                No hay fragmentación externa, porque se simula monoprogramación.
              </p>

              <div class="flex w-full">
                <Button label="Limpiar Memoria" @click="clearMemory">
                  <template #icon>
                    <IconBroom class="w-4" />
                  </template>
                </Button>
              </div>
            </div>
          </div>
        </div>

        <div class="">
          <Map
            :memorySize="data.memorySize"
            :operatingSystem="operatingSystemSelection"
            :process="processSelection"
          />
        </div>
      </div>
    </Window>
  </main>
</template>
