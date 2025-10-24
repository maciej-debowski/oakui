<script lang="ts" setup>
    import Avatar from './Avatar.vue'

    defineProps ({
        users: Array
    })
</script>

<template>
    <div class="avatar-group -space-x-6">
        <Avatar v-for="user in users" :key="user" :id="user.asset_id" :size="8" />
    </div>
</template>