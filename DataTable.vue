<script lang="ts" setup>
    import { computed, onMounted, ref, watch } from 'vue'
    import Axios from '../../axios'
    import Link from './Link.vue'
    import Pagination from './Pagination.vue'

    const { from, display, reloading } = defineProps ({
        display: Array,
        from: String,
        link: String,
        subdomain: String,
        reloading: Boolean,
        casts: {
            type: Object,
            default: {}
        },
        disableTooltipsFor: {
            type: Array,
            default: []
        }
    })

    const displays = computed(() => {
        const headers = []
        const items = []

        console.log(display)

        for(let [a, b] of Object.entries(display))
        {
            headers.push(a)
            items.push(b)
        }

        return [headers, items]
    })

    const formatLink = (link: string, item: Record<string, any>) => {
        return link.replace(/{(\w+)}/g, (_, key) => {
            return item[key] ? encodeURIComponent((item[key].toString() ?? '').replace(/ /g, '_')) : null;
        });
    };

    const tableErrorStatus = ref(false)
    const tableError = ref(null)
    const table = ref([])
    const setTable = (newTable) => table.value = newTable
    const getTable = (page = 1) => Axios.get(`${from}?page=${page}`).then(response => {
        setTable(response.data)
    }).catch(error => {
        tableErrorStatus.value = error.response.status
        tableError.value = error.response
    })

    onMounted(() => {
        getTable()
    })

    watch(() => reloading, async (newVal) => {
        if (newVal) {
            await getTable()
        }
    })
</script>

<template>
    <div class="overflow-x-auto mt-8" v-if="!tableErrorStatus">
        <table class="table table-md">
            <thead>
                <tr>
                    <th></th>
                    <th v-for="(item, key) in displays[0]" :key="key">{{ item }}</th>
                    <th></th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="(item, key) in table.data" :key="key" class="nth-[2n-1]:bg-base-200 hover:bg-base-300">
                    <th>{{ ((table.current_page - 1) * table.per_page) + key + 1 }}</th>
                    <td v-for="(itemKey, key2) in displays[1]" :key="key2">
                        <template v-if="itemKey === 'created_at' || itemKey === 'updated_at'">
                            <span class="tooltip tooltip-top" :data-tip="item[itemKey].split('T')[0]">
                                <slot name="prefix" :title="displays[0][key2]" :value="item[itemKey]" />{{ item[itemKey].split('T')[0] }}
                            </span>
                        </template>

                        <template v-else>
                            <span class="flex items-center justify-start">
                                <template v-if="itemKey === 'status'">
                                    <div class="badge badge-soft badge-success">{{ item[itemKey] }}</div>
                                </template>
                                <template v-else>
                                    <slot name="prefix" :title="displays[0][key2]" :value="item[itemKey]" />
                                    <span 
                                        :data-tip="disableTooltipsFor.includes(itemKey) ? '' : (casts[itemKey] ? casts[itemKey](item[itemKey]) : item[itemKey])" 
                                        class="tooltip tooltip-top" v-html="casts[itemKey] ? casts[itemKey](item[itemKey]) : item[itemKey]">
                                    </span>
                                </template>
                            </span>
                        </template>
                    </td>
                    <td>
                        <Link :href="formatLink(link, item)" :subdomain="subdomain">
                            <slot name="link" />
                        </Link>
                    </td>
                </tr>
            </tbody>
        </table>

        <div v-if="table?.data?.length === 0" class="py-8 flex items-center justify-center">
            No records found
        </div>

        <div class="container_pagination mt-8 w-full flex items-center justify-center" v-if="table?.data?.length > 0">
            <Pagination :table="table" @pageChange="(page) => getTable(page)" />
        </div>
    </div>

    <div class="w-full flex flex-col items-center justify-center py-16" v-else>
        <h2 class="text-3xl mt-24">
            Error {{ tableErrorStatus }}
        </h2>

        <p class="text-center mt-4">
            <span class="block text-error">{{ tableError.data.message }}</span>

            {{ tableError.data.file }}:{{ tableError.data.line }}
        </p>
    </div>
</template>