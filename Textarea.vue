<script lang="ts" setup>
    import { defineProps, defineEmits } from 'vue'

    const props = defineProps<{
        modelValue: string
        name: string
    }>()

    const emit = defineEmits<{
        (e: 'update:modelValue', value: string): void
    }>()

    function updateValue(event: Event) {
        const target = event.target as HTMLInputElement
        emit('update:modelValue', target.value)
        emit('update')
    }
</script>

<template>   
    <textarea
        class="textarea w-full"
        :name="name"
        :id="name"
        :value="modelValue"
        @input="updateValue"
    ></textarea>
</template>
