<template>
    <div :class="class">
        <label :for="id" class="block mb-2 text-sm font-medium text-gray-900 dark:text-white">{{ label }}</label>
        <input :id="id" :name="name" :type="type" :placeholder="placeholder" :value="modelValue"
            @input="$emit('update:modelValue', $event.target.value)" :max="max" :min="min"
            class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500">
        <p class="text-red-500 text-xs italic mt-1" v-if="error">{{ error }}</p>
    </div>
</template>


<script lang="ts" setup>
interface InputFieldProps {
    id: string;
    label: string;
    name: string;
    type?: string;
    modelValue?: string | number;
    error?: string;
    placeholder?: string;
    class?: string;
    min?: number;
    max?: number;
}

const props = withDefaults(defineProps<InputFieldProps>(), {
    type: 'text',
    modelValue: '',
    error: '',
    placeholder: '',
    class: ''
})



</script>
