<script lang="ts" setup>
    import { computed } from "vue"

    const emitter = defineEmits([ "submit" ])

    const { size, theme } = defineProps ({
        size: String,
        theme: String,
        join: {
            type: Boolean,
            default: false
        }
    })

    const computedSize = computed(() => size ? `btn-${size}` : null)
    const computedTheme = computed(() => theme ? `btn-${theme}` : 'btn-primary')

    const emit = () => emitter('submit')
</script>

<template>
    <button :class="`btn btn-soft ${computedTheme} ${computedSize} ${join ? 'join-item' : ''}`" @click="emit">
        <slot />
    </button>
</template>