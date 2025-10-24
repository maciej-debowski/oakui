<script lang="ts" setup>
    import { defineProps, defineEmits } from 'vue'

    const props = defineProps<{
        modelValue: string
        name: string,
        lazy: boolean
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
    <template v-if="lazy">
        <input
            type="text"
            class="input w-full"
            :name="name"
            :id="name"
            :value="modelValue"
            @change="updateValue"
        >
    </template>

    <template v-else>
        <input
            type="text"
            class="input w-full"
            :name="name"
            :id="name"
            :value="modelValue"
            @input="updateValue"
        >
    </template>
</template>
