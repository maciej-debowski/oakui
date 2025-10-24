<script lang="ts" setup>
    import { ref, onMounted } from 'vue'
    import Axios from '../../axios'

    const { url, method } = defineProps ({
        url: String,
        method: String
    })

    const dataErrorStatus = ref(false)

    const loading = ref(false)
    const setLoading = (newValue) => loading.value = newValue

    const data = ref(null)
    const setData = (newData) => data.value = newData
    const getData = () => {
        setLoading(true)

        Axios[method](`${url}`).then(response => {
            setData(response.data)
            setLoading(false)
        }).catch(error => {
            dataErrorStatus.value = error.response.status
            setLoading(false)
        })
    }

    onMounted(() => {
        getData()
    })
</script>

<template>
    <div class="request">
        <template v-if="loading">
            <div class="request__loading flex items-center justify-center">
                <div class="loading loading-spinner"></div>
            </div>
        </template>

        <template v-else>
            <template v-if="dataErrorStatus">
                <slot name="failed" />
            </template>

            <template v-else>
                <slot name="results" :response="data" :reload="getData" />
            </template>
        </template>
    </div>
</template>