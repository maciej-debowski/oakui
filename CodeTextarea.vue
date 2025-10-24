<script lang="ts" setup>
import { defineProps, defineEmits, ref, watch } from 'vue'

const props = defineProps<{
  modelValue: string
  name: string
}>()

const emit = defineEmits<{
  (e: 'update:modelValue', value: string): void
}>()

const textarea = ref<HTMLTextAreaElement | null>(null)
const value = ref(props.modelValue)

watch(
  () => props.modelValue,
  (newVal) => {
    if (newVal !== value.value) value.value = newVal
  }
)

function handleInput(e: Event) {
  const target = e.target as HTMLTextAreaElement
  value.value = target.value
  emit('update:modelValue', target.value)
}

function handleKeydown(e: KeyboardEvent) {
  const el = textarea.value
  if (!el) return

  const { selectionStart, selectionEnd, value: text } = el

  // TAB → wstaw 2 spacje zamiast focusu
  if (e.key === 'Tab') {
    e.preventDefault()
    const before = text.substring(0, selectionStart)
    const after = text.substring(selectionEnd)
    const newText = before + '  ' + after
    el.value = newText
    el.selectionStart = el.selectionEnd = selectionStart + 2
    handleInput(e as unknown as Event)
  }

  // AUTO-DOMYKANIE nawiasów i cudzysłowów
  const pairs: Record<string, string> = {
    '{': '}',
    '[': ']',
    '"': '"'
  }

  if (pairs[e.key]) {
    e.preventDefault()
    const before = text.substring(0, selectionStart)
    const after = text.substring(selectionEnd)
    const insert = e.key + pairs[e.key]
    el.value = before + insert + after
    el.selectionStart = el.selectionEnd = selectionStart + 1
    handleInput(e as unknown as Event)
  }

  // ENTER → automatyczne wcięcie
  if (e.key === 'Enter') {
    e.preventDefault()
    const before = text.substring(0, selectionStart)
    const after = text.substring(selectionEnd)
    // znajdź aktualny poziom wcięcia
    const lineStart = before.lastIndexOf('\n') + 1
    const currentLine = before.substring(lineStart)
    const indentMatch = currentLine.match(/^\s+/)
    const indent = indentMatch ? indentMatch[0] : ''
    const newText = before + '\n' + indent + after
    el.value = newText
    el.selectionStart = el.selectionEnd = selectionStart + 1 + indent.length
    handleInput(e as unknown as Event)
  }
}
</script>

<template>
    <textarea
        ref="textarea"
        class="textarea w-full"
        :name="name"
        :id="name"
        :value="value"
        @input="handleInput"
        @keydown="handleKeydown"
    ></textarea>
</template>
