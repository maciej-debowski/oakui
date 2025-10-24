<script lang="ts" setup>
    import { defineProps, computed } from 'vue'

    const props = defineProps<{
        table: {
            current_page: number
            last_page: number
        }
    }>()

    const emit = defineEmits([ 'pageChange' ])

    const currentPage = computed(() => props.table.current_page)
    const lastPage = computed(() => props.table.last_page)

    const pagesToShow = computed(() => {
        const total = lastPage.value
        const current = currentPage.value
        const maxPages = 5
        let start = Math.max(current - Math.floor(maxPages / 2), 1)
        let end = start + maxPages - 1

        if (end > total) {
            end = total
            start = Math.max(end - maxPages + 1, 1)
        }

        const range = []
        for (let i = start; i <= end; i++) {
            range.push(i)
        }
        return range
    })

    function goToPage(page: number) {
        if (page >= 1 && page <= lastPage.value) {
            emit('pageChange', page)
        }
    }
</script>

<template>
    <div class="join">
        <button
            class="join-item btn btn-sm"
            :disabled="currentPage === 1"
            @click="goToPage(1)"
        >
            &laquo;
        </button>
        <button
            class="join-item btn btn-sm"
            :disabled="currentPage === 1"
            @click="goToPage(currentPage - 1)"
        >
            &lsaquo;
        </button>

        <button
            v-for="page in pagesToShow"
            :key="page"
            @click="goToPage(page)"
            class="join-item btn btn-sm"
            :class="{ 'btn-active': page === currentPage }"
        >
            {{ page }}
        </button>

        <button
            class="join-item btn btn-sm"
            :disabled="currentPage === lastPage"
            @click="goToPage(currentPage + 1)"
        >
            &rsaquo;
        </button>
        <button
            class="join-item btn btn-sm"
            :disabled="currentPage === lastPage"
            @click="goToPage(lastPage)"
        >
            &raquo;
        </button>
    </div>
</template>
