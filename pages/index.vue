<template>
    <div class="flex flex-col items-center container mx-auto py-5">
        <h1 class="text-4xl font-bold mb-4">My Grocery List</h1>

        <div>
            <Button type="button" @click="openModal">Add</Button>
        </div>

        <div class="relative overflow-x-auto shadow-md sm:rounded-lg w-full mt-5">
            <Table :items="items" :headers="['Name', 'Quantity', 'Action']">
                <template #action="{ item }">
                    <button @click="editItem(item)">Edit</button>
                    <button @click="removeItem(items.indexOf(item))">Delete</button>
                </template>
            </Table>
        </div>

        <Modal :is-open="isModalOpen" title="Add Item" :on-save="addItem" :on-cancel="closeModal">
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
const isConfirmDialogOpen = ref(false);
const confirmDialogTitle = ref('');
const confirmDialogMessage = ref('');
let selectedItemIndex = -1;

const editingItem = ref<Item | null>(null);
const storageKey = 'grocery-list';
const showConfirmDialog = ref(false);
const showModal = ref(false);

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
    name.value = '';
    quantity.value = 0;
    isModalOpen.value = true;
}

function closeModal() {
    isModalOpen.value = false;
}

function addItem(item: Item) {
    let uuid = self.crypto.randomUUID();
    const itemWithId = {
        id: uuid,
        name: item.name,
        quantity: item.quantity,
    };
    items.value.push(itemWithId);
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
    showModal.value = true;
}

function saveItem() {
    addItem();
}

function cancelEdit() {
    editingItem.value = null;
    showModal.value = false;
}

</script>