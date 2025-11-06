<template>
  <div class="space-y-6">
    <div class="flex flex-wrap items-center gap-8">

      <!-- Radio CLIENTE -->
      <div>
        <label for="radioCliente"
          class="flex items-center text-sm font-medium text-gray-700 cursor-pointer select-none dark:text-gray-400">
          <div class="relative">
            <input
              type="radio"
              id="radioCliente"
              name="profileSelector"
              value="cliente"
              v-model="selected"
              class="sr-only"
            />
            <div
              :class="selected === 'cliente'
                ? 'border-brand-500 bg-brand-500'
                : 'bg-transparent border-gray-300 dark:border-gray-700'"
              class="mr-3 flex h-5 w-5 items-center justify-center rounded-md border-[1.25px] hover:border-brand-500 dark:hover:border-brand-500"
            >
              <span :class="selected === 'cliente' ? '' : 'opacity-0'">
                <svg width="14" height="14" viewBox="0 0 14 14" fill="none" xmlns="http://www.w3.org/2000/svg">
                  <path d="M11.6666 3.5L5.24992 9.91667L2.33325 7" stroke="white" stroke-width="1.94437"
                    stroke-linecap="round" stroke-linejoin="round" />
                </svg>
              </span>
            </div>
          </div>
          Cliente
        </label>
      </div>

      <!-- Radio DENTISTA -->
      <div>
        <label for="radioDentista"
          class="flex items-center text-sm font-medium text-gray-700 cursor-pointer select-none dark:text-gray-400">
          <div class="relative">
            <input
              type="radio"
              id="radioDentista"
              name="profileSelector"
              value="dentista"
              v-model="selected"
              class="sr-only"
            />
            <div
              :class="selected === 'dentista'
                ? 'border-brand-500 bg-brand-500'
                : 'bg-transparent border-gray-300 dark:border-gray-700'"
              class="mr-3 flex h-5 w-5 items-center justify-center rounded-md border-[1.25px] hover:border-brand-500 dark:hover:border-brand-500"
            >
              <span :class="selected === 'dentista' ? '' : 'opacity-0'">
                <svg width="14" height="14" viewBox="0 0 14 14" fill="none" xmlns="http://www.w3.org/2000/svg">
                  <path d="M11.6666 3.5L5.24992 9.91667L2.33325 7" stroke="white" stroke-width="1.94437"
                    stroke-linecap="round" stroke-linejoin="round" />
                </svg>
              </span>
            </div>
          </div>
          Dentista
        </label>
      </div>

    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'

const selected = ref<'cliente' | 'dentista' | null>(null)
</script>
