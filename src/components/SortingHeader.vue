<template>
    <div class="header">
        <button @click="generateArray" :disabled="disabled">Generate New Array</button>
        <div class="side-container">
            <label for="range">Change Array Size & Speed</label>
            <input
                @change="generateArray"
                :disabled="disabled"
                type="range"
                name="range"
                v-model="range"
                min="4"
                max="100"
                class="slider"
            />
        </div>
        <div>
            <button
                v-for="button in buttons"
                :key="button"
                :class="{ active: sortType === button }"
                @click="selectAlgorithm(button)"
                :disabled="disabled"
            >
                {{ button }}
            </button>

            <button v-if="isAlgorithmSelected" @click="emit('sort', sortType)" :disabled="disabled">Sort!</button>
        </div>
    </div>
</template>

<script setup lang="ts">
import { ref } from 'vue';

const range = 50;

const sortType = ref('');
const isAlgorithmSelected = ref(false);
const buttons = ref(['Bubble Sort', 'Heap Sort', 'Quick Sort', 'Merge Sort']);

defineProps({
    disabled: {
        type: Boolean,
        required: true,
    },
});

const emit = defineEmits<{
    (e: 'sort', sortType: string): void;
    (e: 'generate-array', range: number): void;
}>();

function selectAlgorithm(button: string) {
    sortType.value = button;
    isAlgorithmSelected.value = true;
};

function generateArray(): void {
    emit('generate-array', range);
};
</script>

<style scoped>
input {
    margin: 10px;
}

button {
    margin: 5px;
    cursor: pointer;
}

.active {
    background-color: blueviolet;
}

.slider {
    cursor: pointer;
}

.header {
    background-color: rgb(166, 200, 245);
    display: flex;
}

button:disabled,
input:disabled {
    cursor: default;
}

@media screen and (max-width: 300px) {
    .slider {
        width: 100px;
    }
    .side-container {
        text-align: center;
    }
}
</style>
