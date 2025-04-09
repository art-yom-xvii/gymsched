<template>
    <div
        v-if="isVisible"
        class="bg-opacity-50 fixed inset-0 z-50 flex items-center justify-center bg-black"
    >
        <div class="relative max-h-full max-w-full p-4">
            <img
                :src="imageSrc"
                alt="Zoomed Image"
                class="max-h-full max-w-full object-contain"
            />
            <button
                @click="closeModal"
                class="absolute top-4 right-4 text-2xl font-bold text-white"
            >
                &times;
            </button>
        </div>
    </div>

    <!-- Thumbnail Image -->
    <img v-bind="imageProps" @click="openModal" class="cursor-pointer" />
</template>

<script setup>
import { ref } from 'vue';

// Props
defineProps({
    imageSrc: {
        type: String,
        required: true,
    },
    imageProps: {
        type: Object,
        required: true,
    },
});

// State
const isVisible = ref(false);

// Methods
const openModal = () => {
    isVisible.value = true;
};

const closeModal = () => {
    isVisible.value = false;
};
</script>

<style scoped>
/* Модальное окно */
.fixed {
    z-index: 50;
}
</style>
