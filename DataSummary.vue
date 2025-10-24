<script lang="ts" setup>
    defineProps ({
        label: String,
        value: String
    })
</script>

<template>
    <div class="data-group flex items-center justify-between">
        <span class="text-md">{{ label }}</span>

        <span class="text-md underline">{{ value }}</span>
    </div>
</template>