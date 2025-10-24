<script lang="ts" setup>
    import { ref, nextTick } from 'vue'
    import Button from './Button.vue'
    import FileUpload from './FileUpload.vue'
    import DataFetch from './DataFetch.vue'

    const emit = defineEmits(['update', 'deleteKey'])

    const { attachements } = defineProps ({
        attachements: Array
    })

    const reload = ref(false)
    const update = async (asset) => {
        emit('update', asset)

        reload.value = true
        await nextTick()
        reload.value = false
    }

    const deleteKey = async (id) => {
        emit('deleteKey', id)

        reload.value = true
        await nextTick()
        reload.value = false
    } 
</script>

<template>
    <div class="attachements">
        <div class="attachements-list" v-if="!reload">
            <DataFetch :from="`/assets/build?for=${attachements.join(',')}`" v-if="attachements.length > 0" withoutPaggination>
                <template #block="{ item, refresh }">
                    <div class="attachement w-full flex items-center justify-between bg-base-100 px-2 py-1">
                        <span class="underline text-xs">{{ item.original_name }}</span>

                        <div class="buttons flex gap-2">
                            <a download :href="`/api/assets/${item.uuid}/uuid`">
                                <Button size="xs" theme="success">
                                    <em class="fas fa-download"></em>
                                </Button> 
                            </a>
                            
                            <Button size="xs" theme="error" @submit="() => deleteKey(item.id)">
                                <em class="fas fa-trash"></em>
                            </Button>
                        </div>
                    </div>
                </template>
            </DataFetch>
        </div>

        <FileUpload maxSize="32MB" to="/assets" @update="update" />
    </div>
</template>