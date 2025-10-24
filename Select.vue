<script lang="ts" setup>
import { defineProps, defineEmits } from 'vue'

const props = defineProps<{
  modelValue: string
  name: string,
  size: string
}>()

const emit = defineEmits<{
  (e: 'update:modelValue', value: string): void
  (e: 'update'): void
}>()

function updateValue(event: Event) {
  const target = event.target as HTMLSelectElement
  emit('update:modelValue', target.value)
  emit('update')
}
</script>

<template>
  <select
    :class="`select w-full select-${size}`"
    :name="name"
    :id="name"
    :value="modelValue"
    @change="updateValue"
  >
    <slot />
  </select>
</template>
