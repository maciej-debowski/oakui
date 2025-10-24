<script lang="ts" setup>
    import { ref } from 'vue'
    import Axios from '../../axios'
    import Button from './Button.vue'

    const emit = defineEmits(['update'])

    const { to, maxSize } = defineProps ({
        maxSize: String,
        to: String
    })

    const error = ref(null)
    const setError = (newError) => error.value = newError

    const loading = ref(false)
    const setLoading = (newLoading) => loading.value = newLoading

    const change = (e) => {
        setLoading(true)

        const formData = new FormData()
        formData.append('file', e.target.files[0]);

        Axios.post(to, formData).then((response) => {
            setError(null)
            setLoading(false)

            emit('update', response.data)
        }).catch(error => {
            setError(error)
            setLoading(false)
        })
    }
</script>

<template>
    <template v-if="loading">
        <Button theme="success">
            <div class="loading"></div>
        </Button>
    </template>

    <template v-else>
        <template v-if="error">
            <div class="error-container flex items-center gap-2">
                <Button theme="error">
                    <em class="fas fa-exclamation-triangle"></em> Error occured
                </Button>

                <span class="error-text tooltip tooltip-right text-error" :data-tip="`${error.response.data.file}:${error.response.data.line}`">
                    {{ error.response.data.message }}
                </span>
            </div>
        </template>

        <template v-else>
            <fieldset class="fieldset w-full">
                <input type="file" class="file-input file-input-sm w-full" @change="change" />
            </fieldset>
        </template>
    </template>
</template>