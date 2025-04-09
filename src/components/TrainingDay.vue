<script setup>
import { ref, watch, onMounted } from 'vue';

const props = defineProps({
    day: String,
});

defineEmits(['goBack']);

const newExercise = ref('');
const exercises = ref([]);
let canEditExercises = ref(false);

const storageKey = ref('');

onMounted(() => {
    storageKey.value = `exercises_${props.day}`;

    const saved = localStorage.getItem(storageKey.value);
    if (saved) {
        try {
            exercises.value = JSON.parse(saved);
        } catch (e) {
            console.error('Ошибка чтения localStorage:', e);
        }
    }
});

watch(
    exercises,
    (newVal) => {
        localStorage.setItem(storageKey.value, JSON.stringify(newVal));
    },
    { deep: true },
);

const editExercises = () => {
    canEditExercises.value = !canEditExercises.value;
};

const addExercise = () => {
    if (newExercise.value.trim() !== '') {
        exercises.value.push(newExercise.value.trim());
        newExercise.value = '';
    }
};

const removeExercise = (index) => {
    exercises.value.splice(index, 1);
};
</script>

<template>
    <section class="space-y-6 p-4">
        <div class="mr-2 flex items-center justify-between">
            <h2 class="text-2xl font-bold text-gray-800">
                {{ day }}'s Training
            </h2>

            <button
                @click="editExercises"
                class="text-2xl text-black hover:text-gray-500 focus:outline-none"
                title="Edit Exercises"
            >
                <!-- Если режим редактирования включён -->
                <span v-if="canEditExercises"
                    ><svg
                        xmlns="http://www.w3.org/2000/svg"
                        fill="none"
                        viewBox="0 0 24 24"
                        stroke-width="1.5"
                        stroke="currentColor"
                        class="size-6"
                    >
                        <path
                            stroke-linecap="round"
                            stroke-linejoin="round"
                            d="M9 12.75 11.25 15 15 9.75M21 12a9 9 0 1 1-18 0 9 9 0 0 1 18 0Z"
                        />
                    </svg>
                </span>

                <!-- Если выключен — SVG иконка -->
                <svg
                    v-else
                    xmlns="http://www.w3.org/2000/svg"
                    fill="none"
                    viewBox="0 0 24 24"
                    stroke-width="1.5"
                    stroke="currentColor"
                    class="size-6"
                >
                    <path
                        stroke-linecap="round"
                        stroke-linejoin="round"
                        d="m16.862 4.487 1.687-1.688a1.875 1.875 0 1 1 2.652 2.652L10.582 16.07a4.5 4.5 0 0 1-1.897 1.13L6 18l.8-2.685a4.5 4.5 0 0 1 1.13-1.897l8.932-8.931Zm0 0L19.5 7.125M18 14v4.75A2.25 2.25 0 0 1 15.75 21H5.25A2.25 2.25 0 0 1 3 18.75V8.25A2.25 2.25 0 0 1 5.25 6H10"
                    />
                </svg>
            </button>
        </div>

        <!-- Упражнения -->
        <div>
            <h3 class="mb-2 text-xl font-semibold text-gray-700">Exercises</h3>
            <ul class="space-y-2">
                <span v-if="exercises.length === 0" class="text-gray-500">
                    No exercises added yet.
                </span>
                <li
                    v-for="(exercise, index) in exercises"
                    :key="index"
                    class="flex items-center justify-between gap-2 rounded-lg bg-white p-2 shadow"
                >
                    <!-- Показывать input в режиме редактирования -->
                    <input
                        v-if="canEditExercises"
                        v-model="exercises[index]"
                        type="text"
                        class="flex-1 border-b border-gray-300 px-2 py-1 focus:border-orange-600 focus:outline-none"
                    />

                    <!-- Или просто текст -->
                    <span v-else class="flex-1 px-2 text-gray-800">
                        {{ exercise }}
                    </span>

                    <!-- Delete button -->
                    <button
                        v-show="canEditExercises"
                        class="text-2xl text-red-800 hover:underline"
                        @click="removeExercise(index)"
                    >
                        ×
                    </button>
                </li>
            </ul>

            <div class="mt-3 flex gap-2">
                <input
                    v-model="newExercise"
                    type="text"
                    placeholder="e.g. Squats 3x12"
                    class="flex-1 rounded-md border border-gray-400 p-2"
                />
                <button
                    class="add-btn rounded-md bg-orange-600 px-4 py-2 text-white transition hover:bg-orange-700"
                    @click="addExercise"
                >
                    Add
                </button>
            </div>
        </div>
    </section>
</template>
