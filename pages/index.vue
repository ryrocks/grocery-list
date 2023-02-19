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
            <Table :items="items" :editItem="editItem" :removeItem="removeItem" />
        </div>

        <div v-if="editingItem !== null" class="w-full mb-4 mt-4">
            <label for="edit-name" class="block text-gray-700 font-bold mb-2">Name:</label>
            <input id="edit-name" v-model="editingItem.name" type="text"
                class="appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
                placeholder="Enter item name">
        </div>

        <div v-if="editingItem !== null" class="w-full mb-4">
            <label for="edit-quantity" class="block text-gray-700 font-bold mb-2">Quantity:</label>
            <input id="edit-quantity" v-model.number="editingItem.quantity" type="number"
                class="appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
                placeholder="Enter item quantity">
        </div>

        <div v-if="editingItem !== null" class="w-full mb-4">
            <button class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded mr-2"
                @click="saveItem">Save</button>
            <button class="bg-gray-500 hover:bg-gray-700 text-white font-bold py-2 px-4 rounded"
                @click="cancelEdit">Cancel</button>
        </div>
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
    items.value.splice(index, 1);
}

function editItem(index: number) {
    editingItem.value = { ...items.value[index] };
}

function saveItem() {
    if (editingItem.value) {
        const index = items.value.findIndex(item => item === editingItem.value);
        if (index >= 0) {
            items.value[index] = editingItem.value;
            editingItem.value = null;
        }
    }
}

function cancelEdit() {
    editingItem.value = null;
}
</script>

<style>
/* You can import Tailwind styles in your main.js file */
</style>