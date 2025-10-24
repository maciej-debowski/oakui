<script lang="ts" setup>
    import { computed, onMounted, ref } from 'vue'
    import Axios from '../../axios'
    import Link from './Link.vue'
    import Pagination from './Pagination.vue'
    import TextInput from './TextInput.vue'
    import Button from './Button.vue'

    const { from } = defineProps ({
        from: String,
        withoutPaggination: Boolean,
        buttonHref: String,
        noAdd: Boolean
    })

    const search = ref('')

    const tableErrorStatus = ref(false)
    const loading = ref(false)
    const setLoading = (newValue) => loading.value = newValue
    const table = ref([])
    const setTable = (newTable) => table.value = newTable
    const getTable = (page = 1) => {
        setLoading(true)

        Axios.get(`${from}${from.indexOf('?') !== -1 ? '&' : '?'}page=${page}&search=${search.value}`).then(response => {
            setTable(response.data)
            setLoading(false)
        }).catch(error => {
            tableErrorStatus.value = error.response.status
            setLoading(false)
        })
    }

    onMounted(() => {
        getTable()
    })
</script>

<template>
    <div class="overflow-x-auto mt-4" v-if="!tableErrorStatus">
        <div class="form-search mb-4 px-4 py-1 flex items-center justify-between" v-if="!withoutPaggination">
            <div class="group flex items-center justify-start">
                <div class="w-96">
                    <TextInput placeholder="Search" v-model="search" @update="getTable" />
                </div>

                <div class="loading_box h-full ml-4" v-if="loading">
                    <div class="loading"></div>
                </div>
            </div>

            <a :href="buttonHref" v-if="!noAdd">
                <Button theme="success">
                    Add <em class="fas fa-plus ml-2"></em>
                </Button>
            </a>
        </div>

        <div class="data">
            <div class="data-item mb-4" v-for="item in table.data" :key="item">
                <slot name="block" :item="item" :refresh="getTable" />
            </div>
        </div>

        <div v-if="table?.data?.length === 0" class="py-8 flex items-center justify-center">
            Table has no records
        </div>

        <div class="container_pagination mt-8 w-full flex items-center justify-center" v-if="table?.data?.length > 0 && !withoutPaggination">
            <Pagination :table="table" @pageChange="(page) => getTable(page)" />
        </div>
    </div>

    <div class="w-full flex items-center justify-center" v-else>
        <h2 class="text-3xl">
            Error {{ tableErrorStatus }}
        </h2>
    </div>
</template>