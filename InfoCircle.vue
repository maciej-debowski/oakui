<script lang="ts" setup>
    defineProps ({
        tooltip: String
    })
</script>

<template>
    <div class="tooltip tooltip-top" :data-tip="tooltip">
        <em class="fas fa-exclamation-triangle"></em>
    </div>
</template>