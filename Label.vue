<script lang="ts" setup>
    defineProps ({
        for: String,
        align: String
    })
</script>

<template>
    <label :for="for" :class="`label mb-1 text-${align} block`">
        <slot />    
    </label>
</template>