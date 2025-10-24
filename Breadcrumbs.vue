<script setup lang="ts">
    import Link from './Link.vue'
</script>

<template>
    <div class="breadcrumbs text-sm py-0">
        <ul>
            <li>
                <Link href="/d" subdomain="app"><em class="fas fa-home"></em> OakCloud</Link>
            </li>

            <slot />
        </ul>
    </div>
</template>