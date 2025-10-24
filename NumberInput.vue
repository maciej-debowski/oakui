<script lang="ts" setup>
import { defineProps, defineEmits } from 'vue'

const props = defineProps<{
    modelValue: number
    name: number,
    step: number
}>()

const emit = defineEmits<{
    (e: 'update:modelValue', value: number): void
}>()

function updateValue(event: Event) {
    const target = event.target as HTMLInputElement
    emit('update:modelValue', target.value)
}

function increment() {
    const value = parseFloat(props.modelValue) || 0
    emit('update:modelValue', Number(value + (props.step ?? 1)))
}

function decrement() {
    const value = parseFloat(props.modelValue) || 0
    emit('update:modelValue', Number(value - (props.step ?? 1)))
}
</script>

<template>   
    <div class="join w-full flex items-center justify-between">
        <button 
            type="button"
            class="btn btn-error btn-soft join-item"
            @click="decrement"
        >
            -
        </button>
        <input
            type="number"
            class="input join-item w-full"
            :name="name"
            :id="name"
            :step="step"
            :value="modelValue"
            @input="updateValue"
        >
        <button 
            type="button"
            class="btn btn-success btn-soft join-item"
            @click="increment"
        >
            +
        </button>
    </div>
</template>
