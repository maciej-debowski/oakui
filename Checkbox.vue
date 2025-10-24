<script lang="ts" setup>
    import { defineProps, defineEmits } from 'vue'

    const props = defineProps<{
        modelValue: any
        name: string
    }>()

    const emit = defineEmits<{
        (e: 'update:modelValue', value: string): void
    }>()

    function updateValue(event: Event) {
        const target = event.target as HTMLInputElement
        emit('update:modelValue', target.checked)
        emit('update', target.checked)
    }
</script>

<template>   
    <input
        type="checkbox"
        class="checkbox block"
        :name="name"
        :id="name"
        :value="modelValue === 1 ? true : modelValue"
        :checked="modelValue"
        @input="updateValue"
    >
</template>
