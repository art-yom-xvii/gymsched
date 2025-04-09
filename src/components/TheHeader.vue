<template>
    <header
        class="grid grid-cols-3 items-center bg-gray-900 px-2 py-3 text-white shadow-md"
    >
        <!-- Back Icon -->
        <div>
            <button
                v-if="showBackIcon"
                @click="$emit('goHome')"
                class="text-xl text-white hover:text-gray-200 focus:outline-none"
                title="Go Home"
            >
                ⤶
            </button>
        </div>

        <!-- App Title -->
        <h1 class="col-span-1 text-center text-lg font-semibold">GYMsched</h1>

        <!-- Burger Menu -->
        <div class="flex justify-end">
            <button
                @click="$emit('toggleSidebar')"
                class="text-white hover:text-gray-200 focus:outline-none"
                title="Toggle Menu"
            >
                <svg
                    class="h-6 w-6"
                    fill="none"
                    stroke="currentColor"
                    stroke-width="2"
                    viewBox="0 0 24 24"
                >
                    <path
                        stroke-linecap="round"
                        stroke-linejoin="round"
                        d="M4 6h16M4 12h16M4 18h16"
                    />
                </svg>
            </button>
        </div>
    </header>
</template>

<script setup>
defineProps({
    showBackIcon: Boolean,
});
</script>
