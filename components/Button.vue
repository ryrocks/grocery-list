<template>
    <button :type="type" :class="buttonClass" :disabled="disabled">
        <slot></slot>
    </button>
</template>

<script lang="ts" setup>
interface ButtonProps {
    buttonClass?: string;
    disabled?: boolean;
    type?: 'button' | 'submit' | 'reset';
}

const props = withDefaults(defineProps<ButtonProps>(), {
    buttonClass: 'text-white bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-blue-300 font-medium rounded-lg text-sm w-full sm:w-auto px-5 py-2.5 text-center dark:bg-blue-600 dark:hover:bg-blue-700 dark:focus:ring-blue-800',
    disabled: false,
    type: 'button'
})

</script>