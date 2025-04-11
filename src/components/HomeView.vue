<script setup>
import { ref } from 'vue';

const days = [
    'Monday',
    'Tuesday',
    'Wednesday',
    'Thursday',
    'Friday',
    'Saturday',
    'Sunday',
];
const today = new Date().toLocaleString('en-US', { weekday: 'long' });
const currentDay = ref(today);
</script>

<template>
    <section class="m-auto max-w-[800px] p-4">
        <h1 class="text-3xl font-bold text-gray-800">Hello, Lusine!</h1>
        <p class="mt-4 text-lg text-gray-600">
            Plan and track your workouts easily. Start by selecting a day to
            create your workout routine.
        </p>

        <div class="mt-8">
            <h2 class="text-2xl font-semibold text-gray-700">Training Days</h2>
            <div class="mt-4 grid grid-cols-1 gap-4">
                <button
                    v-for="day in days"
                    :key="day"
                    :class="[
                        'w-full rounded-lg py-4 text-lg font-semibold transition',
                        currentDay === day
                            ? 'bg-orange-600 text-white hover:bg-orange-700'
                            : 'bg-gray-300 text-gray-700 hover:bg-gray-400',
                    ]"
                    @click="$emit('openDay', day)"
                >
                    {{ day }}
                </button>
            </div>
        </div>
    </section>
</template>
