<script lang="ts" setup>
    import { defineProps, defineEmits, ref, watch } from 'vue'

    const emit = defineEmits(['update:modelValue', 'strength'])
    const passwordStrength = ref<'Weak' | 'Medium' | 'Strong'>('Weak')

    const internalValue = ref('')
    const props = defineProps<{ modelValue: string, name: string }>()

    internalValue.value = props.modelValue

    watch(() => props.modelValue, (newVal) => {
        internalValue.value = newVal
    })

    function updateValue(e: Event) {
        const target = e.target as HTMLInputElement
        internalValue.value = target.value
        emit('update:modelValue', target.value)
        emit('strength', passwordStrength.value)
    }

    // Obliczanie siły hasła

    watch(internalValue, (val) => {
        passwordStrength.value = calculateStrength(val)
    })

    function calculateStrength(password: string): 'Weak' | 'Medium' | 'Strong' {
        let score = 0

        if (password.length >= 8) score++
        if (/[A-Z]/.test(password)) score++
        if (/[0-9]/.test(password)) score++
        if (/[^A-Za-z0-9]/.test(password)) score++

        if (score <= 1) return 1
        if (score === 2) return 2
        return 3
    }
</script>

<template>
  <div class="textinput-base w-full">
    <label :class="`input ${passwordStrength < 3 ? 'input-error' : ''} w-full`">
        <svg class="h-[1em] opacity-50" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24">
            <g
            stroke-linejoin="round"
            stroke-linecap="round"
            stroke-width="2.5"
            fill="none"
            stroke="currentColor"
            >
            <path
                d="M2.586 17.414A2 2 0 0 0 2 18.828V21a1 1 0 0 0 1 1h3a1 1 0 0 0 1-1v-1a1 1 0 0 1 1-1h1a1 1 0 0 0 1-1v-1a1 1 0 0 1 1-1h.172a2 2 0 0 0 1.414-.586l.814-.814a6.5 6.5 0 1 0-4-4z"
            ></path>
            <circle cx="16.5" cy="7.5" r=".5" fill="currentColor"></circle>
            </g>
        </svg>
        
        <input
            type="password"
            :name="name"
            :id="name"
            :value="internalValue"
            @input="updateValue"
        />
    </label>

    <p class="text-right text-xs text-error mt-1" v-if="passwordStrength < 3">Password is too weak (requirements: lower case, upper case, number, special character)</p>
  </div>
</template>
