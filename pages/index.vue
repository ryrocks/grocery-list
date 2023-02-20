<template>
    <div class="flex flex-col items-center container mx-auto py-5">
        <h1 class="text-4xl font-bold mb-4">My Grocery List</h1>

        <div>
            <Button type="button" @click="openModal">Add</Button>
        </div>

        <div class="relative overflow-x-auto shadow-md sm:rounded-lg w-full mt-5">
            <Table :items="items" :headers="['Name', 'Quantity', 'Action']" :editItem="editItem" :removeItem="removeItem">
            </Table>
        </div>

        <Modal :isOpen="isModalOpen" :title="editingItem ? 'Edit Item' : 'Add Item'" :onSave="saveItem"
            :onCancel="closeModal">
            <InputField id="name" name="name" label="Name" v-model:modelValue="name" placeholder="Enter name"
                class="mb-4" />
            <InputField id="quantity" name="quantity" label="Quantity" v-model:modelValue="quantity"
                placeholder="Enter quantity" :min="0" type="number" />
        </Modal>

        <ConfirmDialog title="Warning" message="Are you sure you want to delete this item?" :isOpen="showConfirmDialog"
            :onConfirm="removeSelectedItem" :onCancel="cancelRemoveSelectedItem" />
    </div>
</template>

<script lang="ts" setup>

interface Item {
    id: string;
    name: string;
    quantity: number;
}

const items = ref<Item[]>([]);
const name = ref('');
const quantity = ref(0);
const isModalOpen = ref(false);
let selectedItemIndex = -1;

const editingItem = ref<Item | null>(null);
const storageKey = 'grocery-list';
const showConfirmDialog = ref(false);

// Load items from local storage on component mount
onMounted(() => {
    const storedItems = localStorage.getItem(storageKey);
    if (storedItems) {
        items.value = JSON.parse(storedItems);
    }
});

// Save items to local storage whenever the items array changes
watch(items, (newItems) => {
    localStorage.setItem(storageKey, JSON.stringify(newItems));
});


function openModal() {
    if (editingItem.value) {
        name.value = editingItem.value.name;
        quantity.value = editingItem.value.quantity;
    } else {
        name.value = '';
        quantity.value = 0;
    }

    isModalOpen.value = true;
}

function closeModal() {
    editingItem.value = null;
    isModalOpen.value = false;
}

function saveItem(item: Item) {
    if (editingItem.value) {
        const index = items.value.findIndex((i) => i.id === editingItem.value?.id);
        items.value[index] = { ...editingItem.value, ...item };
    } else {
        items.value.push(item);
    }

    closeModal();
}

function removeItem(index: number) {
    showConfirmDialog.value = true;
    selectedItemIndex = index;
}

function removeSelectedItem() {
    items.value.splice(selectedItemIndex, 1);
    showConfirmDialog.value = false;
}

function cancelRemoveSelectedItem() {
    showConfirmDialog.value = false;
}

function editItem(index: number) {
    editingItem.value = { ...items.value[index] };
    openModal();
}

</script>