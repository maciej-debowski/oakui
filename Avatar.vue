<script lang="ts" setup>
    defineProps ({
        id: String,
        size: {
            type: Number,
            default: 16
        }
    })
</script>

<template>
    <div class="avatar">
        <div :class="`w-${size} rounded-full`">
            <img :src="`/api/assets/${id}`" />
        </div>
    </div>
</template>