<script lang="ts" setup>
    import { ref } from 'vue'
    import Axios from '../../axios'
    import Button from './Button.vue'

    const { item, method, to } = defineProps ({
        item: Object,
        to: String,
        method: String,
        theme: {
            type: String,
            default: "success"
        },
        size: String,
    })

    const emit = defineEmits ([
        'success',
        'success.more',
        'failure',
        'failure.more'
    ])

    const loading = ref(false)
    const setLoading = (newLoading) => loading.value = newLoading
    
    const error = ref(false)
    const setError = (newError) => error.value = newError
    
    const saved = ref(false)
    const setSaved = (newSaved) => saved.value = newSaved

    const submit = () => {
        setLoading(true);

        ({...Axios})[method](to, { ...item }).then(response => {
            setError(null)
            setLoading(false)
            setSaved(true)
            emit('success', response)
            emit('success.more')

            setTimeout(() => {
                setSaved(false)
            }, 1000)
        }).catch(error => {
            setError(error)
            setLoading(false)
            emit('failure')
            emit('failure.more')
        })
    }
</script>

<template>
    <template v-if="loading">
        <Button :theme="theme" :size="size">
            <div class="loading"></div>
        </Button>
    </template>

    <template v-else>
        <template v-if="error">
            <div class="error-container flex items-center gap-2">
                <Button :theme="theme" @submit="submit" :size="size">
                    <em class="fas fa-redo"></em>
                </Button>

                <Button theme="error" :size="size">
                    <em class="fas fa-exclamation-triangle"></em> Error occured
                </Button>

                <span class="error-text tooltip tooltip-right text-error" :data-tip="`${error.response.data.file}:${error.response.data.line}`">
                    {{ error.response.data.message }}
                </span>
            </div>
        </template>

        <template v-else>
            <div class="button-container flex items-center gap-2">
                <Button :theme="theme" @submit="submit" :size="size">
                    <template v-if="!saved">
                        <slot />
                    </template>

                    <template v-else>
                        Saved successfully
                    </template>
                </Button>
            </div>
        </template>
    </template>
</template>