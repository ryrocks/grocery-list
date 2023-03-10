<template>
    <div v-if="isOpen" class="fixed z-10 inset-0 overflow-hidden">
        <div class="fixed inset-0 bg-gray-500 bg-opacity-75 z-0"></div>
        <div class="flex items-center justify-center min-h-screen z-10">
            <div class="bg-white rounded-lg overflow-hidden shadow-xl transform transition-all w-4/5 sm:max-w-lg sm:w-full">
                <div class="bg-gray-200 px-4 py-2 flex justify-between items-center">
                    <h1 class="text-lg font-bold">{{ title }}</h1>
                    <button class="text-gray-700 hover:text-gray-900" @click="onCancel">
                        <span class="sr-only">Close</span>
                        <svg class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                                d="M6 18L18 6M6 6l12 12" />
                        </svg>
                    </button>
                </div>
                <div class="p-4">
                    <form ref="form" @submit.prevent="onFormSubmit">
                        <slot></slot>
                        <div class="flex justify-end mt-4">
                            <Button type="button" @click="onCancel" class="mr-2">Cancel</Button>
                            <Button type="submit" class="bg-blue-500 text-white px-4 py-2 rounded hover:bg-blue-600">
                                Save
                            </Button>
                        </div>
                    </form>
                </div>
            </div>
        </div>
    </div>
</template>

<script lang="ts" setup>
interface Item {
    name: string;
    quantity: number;
    id: string;
}

interface ModalProps {
    title?: string
    isOpen?: boolean
    editingItem?: Item | null
    onSave?: (item: Item) => void
    onCancel?: () => void
}

const form = ref(null)

const props = withDefaults(defineProps<ModalProps>(), {
    title: 'Edit Item',
    isOpen: false,
    editingItem: null,
    onSave: (item: Item) => { },
    onCancel: () => { }
});

watch(() => props.isOpen, (value) => {
    value === true ? document.body.style.overflow = 'hidden' : document.body.style.overflow = 'auto'
})

function onFormSubmit() {
    const formElement = form.value;
    if (!formElement) return;

    const formData = new FormData(formElement);
    const name = formData.get('name') as string;
    const quantity = Number(formData.get('quantity'));
    let uuid = props.editingItem ? props.editingItem.id : self.crypto.randomUUID();
    const item: Item = {
        name,
        quantity,
        id: uuid,
    };

    props.onSave(item);
}

</script>

