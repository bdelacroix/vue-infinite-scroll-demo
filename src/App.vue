<script setup lang="ts">
import { ref } from 'vue'
import { useInfiniteScroll, useVirtualList } from '@vueuse/core'

const data = ref(Array.from(Array(10).keys(), (value: number) => `item : ${value}`))
const { list, containerProps, wrapperProps } = useVirtualList(data, { itemHeight: 96 })

useInfiniteScroll(
  containerProps.ref, 
  () => {
    /** Add items to the list on scroll */
    data.value.push(...Array.from(Array(10).keys(), (value: number) => `loaded item : ${value}`))
  },
  { distance: 10 }
)
</script>

<template>
    <div v-bind="containerProps" class="h-screen p-2">
      <div v-bind="wrapperProps" class="max-w-sm mx-auto">
        <div 
          v-for="{ index, data } in list"
          :key="index"
          class="h-[80px] mb-4 flex items-center justify-center bg-green-400 rounded-lg">
          <p>{{ data }}</p>
        </div>
      </div>
    </div>
</template>

