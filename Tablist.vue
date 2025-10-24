<script lang="ts" setup>
    import Link from './Link.vue'
    
    defineProps ({
        tabs: Array,
        current: String
    })
</script>

<template>
    <div role="tablist" class="tabs tabs-border mt-4">
        <template v-for="tab in tabs" :key="tab">
            <Link package subdomain="app" :href="tab.href">
                <a role="tab" :class="`tab ${tab.label === current ? 'tab-active' : ''}`">
                    {{ tab.label }}
                </a>
            </Link>
        </template>
    </div>
</template>