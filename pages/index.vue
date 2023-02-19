<template>
    <div class="flex flex-col items-center container  mx-auto">
        <h1 class="text-4xl font-bold mb-4">My Grocery List</h1>

        <form class="flex items-end" @submit="addItem">
            <InputField id="name" label="Name:" name="name" :modelValue="name"
                @update:modelValue="newValue => name = newValue" placeholder="Enter item name" class="w-full " />

            <InputField type="number" id="quantity" label="Quantity:" name="quantity" :modelValue="quantity"
                @update:modelValue="newValue => quantity = newValue" placeholder="Enter item quantity" class="w-full "
                :min="0" />
            <div>
                <Button type="submit">Add</Button>
            </div>
        </form>

        <div class="relative overflow-x-auto shadow-md sm:rounded-lg w-full mt-5">
            <Table :items="items" :editItem="editItem" :removeItem="removeItem" :headers="['Name', 'Quantity', 'Action']" />
        </div>


        <ConfirmDialog title="Warning" message="Are you sure you want to delete this item?" :isOpen="showConfirmDialog"
            :onConfirm="removeSelectedItem" :onCancel="cancelRemoveSelectedItem" />
    </div>
</template>

<script lang="ts" setup>


interface Item {
    name: string;
    quantity: number;
}

const items = ref<Item[]>([]);
const name = ref('');
const quantity = ref(0);
const editingItem = ref<Item | null>(null);
const storageKey = 'grocery-list';
const showConfirmDialog = ref(false);
let selectedItemIndex = 0;


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

function addItem($event: Event) {
    $event.preventDefault();
    if (name.value.trim() !== '' && quantity.value > 0) {
        items.value.push({
            name: name.value.trim(),
            quantity: quantity.value,
        });
        name.value = '';
        quantity.value = 0;
    }
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
}

// function saveItem() {
//     if (editingItem.value) {
//         const index = items.value.findIndex(item => item === editingItem.value);
//         if (index >= 0) {
//             items.value[index] = editingItem.value;
//             editingItem.value = null;
//         }
//     }
// }

// function cancelEdit() {
//     editingItem.value = null;
// }
</script>
