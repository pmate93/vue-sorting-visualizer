<template>
    <div class="container">
        <div
            v-for="(number, index) in numsArray"
            :key="index"
            :style="{
                height: 5 + number.number + '%',
                width: 100 / numsArray.length + 'vh',
            }"
            :class="[active(index, number)]"
        >
            {{ numsArray.length < 40 ? number.number : null }}
        </div>
    </div>
</template>

<script setup lang="ts">
import { Algorithms } from './constants';

const props = defineProps({
    numsArray: {
        type: Array,
        required: true,
    },
    value_i: {
        type: Number,
        required: true,
    },
    value_j: {
        type: Number,
        required: true,
    },
    value_largest: {
        type: Number,
        required: true,
    },
    pivot: {
        type: Number,
        required: true,
    },
    done: {
        type: Boolean,
        required: true,
    },
    turnRed: {
        type: Boolean,
        required: true,
    },
    doneGreen: {
        type: Boolean,
        required: true,
    },
    counter: {
        type: Number,
        required: true,
    },
    sortType: {
        type: String,
        required: true,
    },
    finalArray: {
        type: Array,
        required: true,
    },
});

function active(index: number, number: any): string {
    if (props.sortType === Algorithms.BubbleSort || props.sortType === '') {
        if (props.doneGreen) {
            return 'active';
        }
        if (props.done) {
            return 'purple';
        }
        if (props.counter <= index && props.counter >= 0) {
            return 'purple';
        } else if (props.value_i === index || props.value_j === index) {
            if (props.turnRed) {
                return 'red';
            }
            return 'active';
        } else {
            return 'element';
        }
    } else if (props.sortType === 'Merge Sort') {
        if (props.doneGreen) {
            return 'active';
        }
        if (props.done) {
            return 'purple';
        }
        if (props.value_i === number.id || props.value_j === number.id) {
            if (props.turnRed) {
                return 'red';
            }
            return 'active';
        } else {
            return 'element';
        }
    } else if (props.sortType === 'Quick Sort') {
        if (props.doneGreen) {
            return 'active';
        }
        if (props.done) {
            return 'purple';
        }

        if (props.pivot === index) {
            if ((props.value_i === props.pivot || props.value_j === props.pivot) && props.turnRed) {
                return 'red';
            }
            if (props.value_i === props.pivot || props.value_j === props.pivot) {
                return 'active';
            }
            if (
                (props.turnRed && props.value_i === props.pivot) ||
                (props.turnRed && props.value_j === props.pivot)
            ) {
                return 'red';
            }
            return 'orange';
        }

        if (props.value_i === index || props.value_j === index) {
            if (props.turnRed) {
                return 'red';
            }
            return 'active';
        } else {
            if (props.numsArray[index] === props.finalArray[index] && props.counter >= 0) {
                return 'purple';
            }
            return 'element';
        }
    } else if (props.sortType === 'Heap Sort') {
        if (props.doneGreen) {
            return 'active';
        }
        if (props.done) {
            return 'purple';
        }
        if (props.counter <= index && props.counter >= 0) {
            return 'purple';
        }

        if (props.value_i === index || props.value_j === index || props.value_largest === index) {
            if (props.turnRed && props.value_largest !== -1) {
                if (props.value_largest === index || props.value_i === index) {
                    return 'red';
                }
                return 'active';
            } else if (props.turnRed && props.value_largest === -1) {
                if (props.value_j === index || props.value_i === index) {
                    return 'red';
                }
                return 'active';
            }
            return 'active';
        } else {
            return 'element';
        }
    }
    return '';
}
</script>

<style scoped>
.orange {
    margin: 0.3%;
    background-color: rgb(241, 146, 111);
}
.purple {
    margin: 0.3%;
    background-color: purple;
}
.red {
    margin: 0.3%;
    background-color: rgb(247, 35, 35);
}
.active {
    margin: 0.3%;
    background-color: green;
}
.element {
    width: 20px;
    background-color: rgb(65, 65, 236);
    margin: 0.3%;
    height: 12%;
}
.container {
    height: 50vh;
    max-width: 60%;
    min-width: 260px;
    margin: auto;
    display: flex;
    justify-content: center;
    color: white;
    text-align: center;

    font-size: 1.1vw;
}
</style>
