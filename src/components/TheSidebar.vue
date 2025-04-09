<template>
    <aside
        class="fixed inset-0 z-50 flex items-center justify-center bg-gray-900 text-white"
        :class="{ '-translate-x-full': !visible, 'translate-x-0': visible }"
    >
        <nav class="text-center">
            <ul class="space-y-6">
                <li>
                    <a
                        href="#"
                        class="block text-2xl font-semibold hover:text-gray-300"
                        @click="$emit('goHome')"
                    >
                        🏠 Home
                    </a>
                </li>
                <li>
                    <a
                        href="#"
                        class="block text-2xl font-semibold hover:text-gray-300"
                    >
                        🏋️‍♂️ My Training Plan
                    </a>
                </li>
                <li>
                    <a
                        href="#"
                        class="block text-2xl font-semibold hover:text-gray-300"
                    >
                        ⚙️ Settings
                    </a>
                </li>
            </ul>
        </nav>
        <button
            @click="$emit('closeSidebar')"
            class="absolute top-4 right-4 text-3xl font-bold text-white hover:text-gray-300"
        >
            &times;
        </button>
    </aside>
</template>

<script setup>
defineProps({
    visible: Boolean,
});
</script>
