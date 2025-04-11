<script setup>
import { ref, watch, onMounted } from 'vue';

const props = defineProps({
    day: String,
});

defineEmits(['goBack']);

const exercises = ref([]);
const newExercise = ref('');
const dietGoals = ref([]);
const newDiet = ref('');
let canEdit = ref(false);

const storageKey = ref('');

onMounted(() => {
    storageKey.value = `exercises_${props.day}`;

    const savedExercises = localStorage.getItem(storageKey.value);
    if (savedExercises) {
        try {
            exercises.value = JSON.parse(savedExercises);
        } catch (e) {
            console.error('Ошибка чтения exercises из localStorage:', e);
        }
    }

    const savedDietGoals = localStorage.getItem(`dietGoals_${props.day}`);
    if (savedDietGoals) {
        try {
            dietGoals.value = JSON.parse(savedDietGoals);
        } catch (e) {
            console.error('Ошибка чтения diet из localStorage:', e);
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

watch(
    dietGoals,
    (newVal) => {
        localStorage.setItem(`dietGoals_${props.day}`, JSON.stringify(newVal));
    },
    { deep: true },
);

const editExercises = () => {
    canEdit.value = !canEdit.value;
};

const addExerciseOrDietGoals = (
    newExerciseOrDietGoals,
    exerciseOrDietGoalsArray,
) => {
    if (newExerciseOrDietGoals.value.trim() !== '') {
        exerciseOrDietGoalsArray.value.push(
            newExerciseOrDietGoals.value.trim(),
        );
        newExerciseOrDietGoals.value = '';
    }
};

const addExercise = () => addExerciseOrDietGoals(newExercise, exercises);
const addDietGoals = () => addExerciseOrDietGoals(newDiet, dietGoals);

const removeExercise = (index) => {
    exercises.value.splice(index, 1);
};

const removeDietGoal = (index) => {
    dietGoals.value.splice(index, 1);
};
</script>

<template>
    <section class="space-y-6 p-4">
        <div class="flex items-center justify-between">
            <h2 class="text-2xl font-bold text-gray-800">
                {{ day }}'s Training
            </h2>

            <button
                @click="editExercises"
                class="size-8 text-black hover:text-gray-500 focus:outline-none"
                title="Edit Exercises"
            >
                <!-- Если режим редактирования включён -->
                <span v-if="canEdit"
                    ><svg
                        xmlns="http://www.w3.org/2000/svg"
                        fill="none"
                        viewBox="0 0 24 24"
                        stroke-width="1.5"
                        stroke="currentColor"
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
                >
                    <path
                        stroke-linecap="round"
                        stroke-linejoin="round"
                        d="m16.862 4.487 1.687-1.688a1.875 1.875 0 1 1 2.652 2.652L10.582 16.07a4.5 4.5 0 0 1-1.897 1.13L6 18l.8-2.685a4.5 4.5 0 0 1 1.13-1.897l8.932-8.931Zm0 0L19.5 7.125M18 14v4.75A2.25 2.25 0 0 1 15.75 21H5.25A2.25 2.25 0 0 1 3 18.75V8.25A2.25 2.25 0 0 1 5.25 6H10"
                    />
                </svg>
            </button>
        </div>

        <div class="flex flex-col gap-20">
            <!-- Упражнения -->
            <div>
                <h3 class="mb-2 text-xl font-semibold text-gray-700">
                    Exercises
                </h3>
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
                            v-if="canEdit"
                            v-model="exercises[index]"
                            type="text"
                            class="min-h-8 flex-1 border-b border-gray-300 px-2 focus:border-orange-600/50 focus:outline-none"
                        />

                        <!-- Или просто текст -->
                        <span
                            v-else
                            class="justify-centeritems-center min-h-8 flex-1 px-2 py-1 text-gray-800"
                        >
                            {{ exercise }}
                        </span>

                        <!-- Delete button -->
                        <button
                            v-show="canEdit"
                            class="h-full w-8 text-xl text-red-800 hover:underline"
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

            <!-- Диета -->
            <div>
                <h3 class="mb-2 text-xl font-semibold text-gray-700">
                    Diet Goals
                </h3>
                <ul class="space-y-2">
                    <span v-if="dietGoals.length === 0" class="text-gray-500">
                        No diet goals added yet.
                    </span>
                    <li
                        v-for="(dietItem, index) in dietGoals"
                        :key="index"
                        class="flex items-center justify-between gap-2 rounded-lg bg-white p-2 shadow"
                    >
                        <!-- Показывать input в режиме редактирования -->
                        <input
                            v-if="canEdit"
                            v-model="dietGoals[index]"
                            type="text"
                            class="min-h-8 flex-1 border-b border-gray-300 px-2 focus:border-orange-600/50 focus:outline-none"
                        />

                        <!-- Или просто текст -->
                        <span
                            v-else
                            class="justify-centeritems-center min-h-8 flex-1 px-2 py-1 text-gray-800"
                        >
                            {{ dietItem }}
                        </span>

                        <!-- Delete button -->
                        <button
                            v-show="canEdit"
                            class="h-full w-8 text-xl text-red-800 hover:underline"
                            @click="removeDietGoal(index)"
                        >
                            ×
                        </button>
                    </li>
                </ul>

                <div class="mt-3 flex gap-2">
                    <input
                        v-model="newDiet"
                        type="text"
                        placeholder="e.g. Carbs 10g"
                        class="flex-1 rounded-md border border-gray-400 p-2"
                    />
                    <button
                        class="add-btn rounded-md bg-orange-600 px-4 py-2 text-white transition hover:bg-orange-700"
                        @click="addDietGoals"
                    >
                        Add
                    </button>
                </div>
            </div>
        </div>
    </section>
</template>
