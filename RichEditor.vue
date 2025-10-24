<script setup>
    import { ref, watch } from 'vue'
    import { useEditor, EditorContent } from '@tiptap/vue-3'
    import StarterKit from '@tiptap/starter-kit'
    import Underline from '@tiptap/extension-underline'
    import Link from '@tiptap/extension-link'
    import CodeBlockLowlight from '@tiptap/extension-code-block-lowlight'
    import Highlight from '@tiptap/extension-highlight'
    import Placeholder from '@tiptap/extension-placeholder'
    import CharacterCount from '@tiptap/extension-character-count'
    import { common, createLowlight } from 'lowlight'

    const props = defineProps({
        modelValue: String
    })

    const emit = defineEmits(['update:modelValue', 'update'])
    const lowlight = createLowlight(common)

    const editor = useEditor({
        content: props.modelValue,
        extensions: [
            StarterKit,
            Underline,
            Link.configure({
                openOnClick: true,
            }),
            CodeBlockLowlight.configure({
                lowlight,
            }),
            Highlight,
            Placeholder.configure({
                placeholder: 'Start typing...',
            }),
            CharacterCount,
        ],
    })

    const lastEmitted = ref(props.modelValue)

    function emitContentThrottled() {
        if (!editor.value) return
        
        const html = editor.value.getHTML()
        if (html !== lastEmitted.value) {
            lastEmitted.value = html
            emit('update:modelValue', html)
            emit('update')
        }
    }

    // Use a 50ms throttling timer
    let throttleTimeout = null

    watch(
        () => editor.value?.getHTML(),
        () => {
            if (throttleTimeout) clearTimeout(throttleTimeout)
                throttleTimeout = setTimeout(() => {
                emitContentThrottled()
            }, 50)
        }
    )
</script>

<template>
    <div class="rich-editor textarea w-full">    
        <div class="toolbar join block mb-4">
            <button class="btn btn-sm join-item" :class="{ 'btn-primary': editor?.isActive('bold') }" @click="editor.chain().focus().toggleBold().run()">
                Bold
            </button>

            <button class="btn btn-sm join-item" :class="{ 'btn-primary': editor?.isActive('underline') }" @click="editor.chain().focus().toggleUnderline().run()">
                Underline
            </button>

            <button class="btn btn-sm join-item" :class="{ 'btn-primary': editor?.isActive('link') }" @click="setLink">
                Link
            </button>

            <button class="btn btn-sm join-item" @click="editor.chain().focus().toggleCodeBlock().run()">
                Code Block
            </button>

            <button class="btn btn-sm join-item" :class="{ 'btn-primary': editor?.isActive('highlight') }" @click="editor.chain().focus().toggleHighlight().run()">
                Highlight
            </button>
        </div>

        <div class="editor_service">
            <editor-content :editor="editor" />
        </div>
    </div>
</template>

<style lang="scss">
    .ProseMirror-focused {
        outline: 0px !important;
        border: none !important;
        box-shadow: none !important;
    }

    .tiptap {
        height: 100%;
    }

    .editor_service {
        height: 100%;
    }

    .editor_service div:not(.tiptap) {
        height: 100%;
    }
</style>
