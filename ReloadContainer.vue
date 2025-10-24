<script setup>
    import { nextTick, ref } from 'vue';

    const reloading = ref(false)
    const reloadIndex = ref(Math.random())

    const reload = async (time) => {
        setTimeout(async () => {
            reloading.value = true
            await nextTick()
            reloading.value = false

            reloadIndex.value = parseInt(Math.random()*1e20).toString(36)
        }, time)
    }
</script>

<template>
    <template v-if="!reloading">
        <slot name="reload" :refresh="reload" :reloadIndex="reloadIndex" />
    </template>
</template>