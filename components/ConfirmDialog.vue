<template>
    <div v-if="isOpen" class="fixed z-10 inset-0 overflow-hidden">
        <div class="fixed inset-0 bg-gray-500 bg-opacity-75 z-0"></div>
        <div class="flex items-center justify-center min-h-screen">
            <div class="bg-white rounded-lg overflow-hidden shadow-xl transform transition-all w-4/5 sm:max-w-lg sm:w-full">
                <div class="bg-amber-500 px-4 py-2">
                    <h1 class="text-lg font-bold text-white">{{ title }}</h1>
                </div>
                <div class="p-4">
                    <p class="mb-4">{{ message }}</p>
                    <div class="flex justify-end space-x-4">
                        <button class="bg-amber-500 text-white px-4 py-2 rounded hover:bg-red-600"
                            @click="handleConfirm">Yes</button>
                        <button class="bg-gray-300 text-gray-700 px-4 py-2 rounded hover:bg-gray-400"
                            @click="handleCancel">No</button>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script lang="ts" setup>

interface ConfirmDialogProps {
    title?: string
    message?: string
    onConfirm?: () => void
    onCancel?: () => void
    isOpen?: boolean
    variant?: 'default' | 'danger' | 'warning' | 'success'
}

const props = withDefaults(defineProps<ConfirmDialogProps>(), {
    title: '',
    message: '',
    onConfirm: () => { },
    onCancel: () => { },
    isOpen: false,
    variant: 'default'
})

const isOpen = ref(false)

watch(() => props.isOpen, (value) => {
    isOpen.value = value
    value === true ? document.body.style.overflow = 'hidden' : document.body.style.overflow = 'auto'
})

function handleConfirm() {
    props.onConfirm?.()
    isOpen.value = false
}

function handleCancel() {
    props.onCancel?.()
    isOpen.value = false
}

</script>


