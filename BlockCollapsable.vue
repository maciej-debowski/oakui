<template>
    <details class="collapse collapse-arrow bg-base-200 rounded-box shadow-md w-full">
        <summary class="collapse-title collapse-arrow font-semibold">
            <slot name="top" />
        </summary>
        <div class="collapse-content text-sm bg-base-300 pt-4">
            <slot name="content" />
        </div>
    </details>
</template>
