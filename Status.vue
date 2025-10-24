<script lang="ts" setup>
    defineProps ({
        status: String
    })
</script>

<template>
    <div class="flex items-center justify-center">
        <div class="badge tooltip tooltip-left" data-tip="Dev stage">{{ status }}</div>
    </div>
</template>