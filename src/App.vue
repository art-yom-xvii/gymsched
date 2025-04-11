<script setup>
import { ref } from 'vue';
import TheHeader from './components/TheHeader.vue';
import TheSidebar from './components/TheSidebar.vue';
import HomeView from './components/HomeView.vue';
import TrainingDay from './components/TrainingDay.vue';

const showSidebar = ref(false);
const currentView = ref('home');
const currentDay = ref('');

function toggleSidebar() {
    showSidebar.value = !showSidebar.value;
}

function goHome() {
    currentView.value = 'home';
    currentDay.value = '';
    showSidebar.value = false;
}

function openDay(day) {
    currentDay.value = day;
    currentView.value = 'day';
}

function goBack() {
    currentView.value = 'home';
}
</script>

<template>
    <div class="min-h-screen bg-gray-100 font-sans text-gray-800">
        <TheHeader
            @toggleSidebar="toggleSidebar"
            @goHome="goHome"
            :showBackIcon="currentView !== 'home'"
        />

        <TheSidebar
            :visible="showSidebar"
            @goHome="goHome"
            @closeSidebar="toggleSidebar"
        />

        <!-- Main Content -->
        <main class="p-4">
            <HomeView v-if="currentView === 'home'" @openDay="openDay" />
            <TrainingDay v-else :day="currentDay" @goBack="goBack" />
        </main>
    </div>
</template>
