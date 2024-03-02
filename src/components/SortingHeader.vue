<template>
    <div class="header">
        <button @click="$emit('generate-array', range)" :disabled="this.disabled">Generate New Array</button>
        <div class="slidecontainer">
            <label for="range">Change Array Size & Speed</label>
            <input
                @change="$emit('generate-array', range)"
                :disabled="this.disabled"
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
                @click="selectSortType(button)"
                :disabled="this.disabled"
            >
                {{ button }}
            </button>

            <button v-if="active" @click="$emit('sort', sortType)" :disabled="this.disabled">Sort!</button>
        </div>
    </div>
</template>

<script>
export default {
    name: 'sorting-header',
    props: {
        disabled: Boolean,
    },

    data() {
        return {
            range: 50,
            sortType: '',
            buttons: ['Bubble Sort', 'Heap Sort', 'Quick Sort', 'Merge Sort'],
            active: false,
        };
    },

    methods: {
        selectSortType(button) {
            this.sortType = button;
            this.active = true;
        },
    },
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
    .slidecontainer {
        text-align: center;
    }
}
</style>
