<script lang="ts" setup>
    defineProps ({
        theme: String
    })
</script>

<template>
    <div :class="`w-full rounded-box p-2 bg-base-200 flex flex-row items-center text-${theme}`">
        <div class="icon flex items-center justify-center mr-3 rounded-box">
            <slot name="icon" />
        </div>

        <slot name="message" />
    </div>
</template>