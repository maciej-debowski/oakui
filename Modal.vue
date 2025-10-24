<script lang="ts" setup>
    const close = () => window.location.hash = ''
</script>

<template>
    <dialog class="modal" role="dialog">
        <div class="modal-box shadow-[inset_0px_0px_0px_1px_#fffaed2d]">
            <slot name="content" :hide="close" />
        </div>

        <form method="dialog" class="modal-backdrop" @click="close">
            <button>close</button>
        </form>
    </dialog>
</template>