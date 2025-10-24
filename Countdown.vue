<script setup>
    import { ref, onMounted, onUnmounted } from 'vue'

    const { to } = defineProps ({
        to: String
    })

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

        timeLeft.value = { days, hours, minutes, seconds }
    }

    onMounted(() => {
        updateCountdown()
        timerId = setInterval(updateCountdown, 1000)
    })

    onUnmounted(() => {
        clearInterval(timerId)
    })
</script>

<template>
    <div class="flex gap-4">
        <div class="flex flex-col text-center">
            <span class="countdown font-mono text-5xl">
                <span :style="`--value:${timeLeft.days}`" aria-live="polite">{{ timeLeft.days }}</span>
            </span>
            days
        </div>
        <div class="flex flex-col text-center">
            <span class="countdown font-mono text-5xl">
                <span :style="`--value:${timeLeft.hours}`" aria-live="polite">{{ timeLeft.hours }}</span>
            </span>
            hours
        </div>
        <div class="flex flex-col text-center">
            <span class="countdown font-mono text-5xl">
                <span :style="`--value:${timeLeft.minutes}`" aria-live="polite">{{ timeLeft.minutes }}</span>
            </span>
            min
        </div>
        <div class="flex flex-col text-center">
            <span class="countdown font-mono text-5xl">
                <span :style="`--value:${timeLeft.seconds}`" aria-live="polite">{{ timeLeft.seconds }}</span>
            </span>
            sec
        </div>
    </div>
</template>