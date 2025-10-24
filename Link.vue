<script lang="ts" setup>
    import { computed } from "vue"
    import { Link } from '@inertiajs/vue3'

    const { href, subdomain } = defineProps ({
        href: String,
        subdomain: {
            type: String,
            default: null
        },
        package: {
            type: Boolean,
            default: false
        }
    })

    const computedHref = computed(() => {
        if (subdomain && false) {
            try {
                const url = new URL(href);
                const parts = url.hostname.split('.');
                
                // Remove the current subdomain if it exists
                if (parts.length > 2) {
                    parts.shift();
                }
                
                url.hostname = `${subdomain}.${parts.join('.')}`;
                return url.toString();
            } catch (error) {
                return href; // fallback if href is not a valid URL
            }
        }
        return href;
    });
</script>

<template>
    <Link :class="`link ${package ? '[text-decoration:_none]' : ''}`" :href="computedHref">
        <slot />
    </Link>
</template>