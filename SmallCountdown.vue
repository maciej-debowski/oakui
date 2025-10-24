<script setup>
    import { ref, onMounted, onUnmounted } from 'vue'

    const { to } = defineProps ({
        to: String
    })

    const emit = defineEmits([ 'finish' ])

    const timeLeft = ref({ days: 0, hours: 0, minutes: 0, seconds: 0 })
    let timerId = null

    const updateCountdown = () => {
        const now = new Date()
        const target = new Date(to.replace(" ", "T")) // ISO-friendly
        const diff = target - now

        if (diff <= 0) {
            timeLeft.value = { days: 0, hours: 0, minutes: 0, seconds: 0 }
            clearInterval(timerId)
            return
        }

        const totalSeconds = Math.floor(diff / 1000)
        const days = Math.floor(totalSeconds / (3600 * 24))
        const hours = Math.floor((totalSeconds % (3600 * 24)) / 3600)
        const minutes = Math.floor((totalSeconds % 3600) / 60)
        const seconds = totalSeconds % 60

        if(totalSeconds <= 0) emit('finish')

        timeLeft.value = { days, hours, minutes, seconds }
    }

    const format = (number) => number < 10 ? `0${number}` : number

    onMounted(() => {
        updateCountdown()
        timerId = setInterval(updateCountdown, 1000)
    })

    onUnmounted(() => {
        clearInterval(timerId)
    })
</script>

<template>
    <div class="inline">
        <span>{{ format(timeLeft.hours) }}:{{ format(timeLeft.minutes) }}:{{ format(timeLeft.seconds) }}</span>
    </div>
</template>