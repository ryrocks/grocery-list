<template>
    <table class="w-full text-sm text-left text-gray-500 dark:text-gray-400">
        <thead class="text-xs text-gray-700 uppercase bg-gray-50 dark:bg-gray-700 dark:text-gray-400">
            <tr>
                <th scope="col" class="px-6 py-3" v-for="(item, index) in headers" :key="index">
                    {{ item }}
                </th>
            </tr>
        </thead>
        <tbody>
            <template v-for="(item, index) in items" :key="item.id">
                <tr
                    :class="{ 'bg-white border-b dark:bg-gray-900 dark:border-gray-700': index % 2 == 0, 'bg-gray-100 border-b dark:bg-gray-800 dark:border-gray-700': index % 2 !== 0 }">
                    <td scope="row" class="px-6 py-4 font-medium text-gray-900 whitespace-nowrap dark:text-white">{{
                        item.name }}</td>
                    <td class="px-6 py-4">{{ item.quantity }}</td>
                    <td class="px-6 py-4">
                        <button class="bg-yellow-500 hover:bg-yellow-700 text-white font-bold py-1 px-2 rounded mr-2"
                            @click="editItem(index)">Edit</button>
                        <button class="bg-red-500 hover:bg-red-700 text-white font-bold py-1 px-2 rounded"
                            @click="removeItem(index)">Remove</button>
                    </td>
                </tr>
            </template>
        </tbody>
    </table>
</template>

<script lang="ts" setup>

interface TableProps {
    items?: {
        name: string
        quantity: number
        id: string
    }[]
    editItem?: (index: number) => void
    removeItem?: (index: number) => void
    headers?: string[]
}
const props = withDefaults(defineProps<TableProps>(), {
    items: () => [],
    editItem: () => { },
    removeItem: () => { },
    headers: () => []
})

</script>