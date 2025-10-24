<script lang="ts" setup>
    defineProps ({
        priority: String
    })
</script>

<template>
    <div class="flex items-center justify-center">
        <span class="mr-2">Priority</span>

        <div v-if="priority === 'Low'" class="badge badge-success tooltip" data-tip="Task priority">Low</div>
        <div v-else-if="priority === 'Medium'" class="badge badge-info tooltip" data-tip="Task priority">Medium</div>
        <div v-else-if="priority === 'High'" class="badge badge-warning tooltip" data-tip="Task priority">High</div>
        <div v-else-if="priority === 'Critical'" class="badge badge-error tooltip" data-tip="Task priority">Critical</div>
    </div>
</template>