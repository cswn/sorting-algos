<script setup lang="ts">
import AlgoVisualizationBarChart from '@/components/algo-visualization/AlgoVisualizationBarChart.vue';
import { Algorithm } from '@/services/types';
import { onMounted, ref } from 'vue';
import bblSort from '@/algorithms/bubbleSort';
import selectionSort from '@/algorithms/selectionSort';
import mergeSort from '@/algorithms/mergeSort';
import insertionSort from '@/algorithms/insertionSort';

const props = defineProps<{
    algorithm: Algorithm,
    description: string
}>();

const numbersToSort = ref([] as Array<number>);
const numbersToSortLength = ref(10);

const startSort = async () => {
    if (props.algorithm === Algorithm.bubble) {
        numbersToSort.value = await bblSort(numbersToSort.value);
    } else if (props.algorithm === Algorithm.selection) {
        numbersToSort.value = await selectionSort(numbersToSort.value);
    } else if (props.algorithm === Algorithm.merge) {
        numbersToSort.value = await mergeSort(numbersToSort.value);
    } else if (props.algorithm === Algorithm.insertion) {
        numbersToSort.value = await insertionSort(numbersToSort.value);
    } else {
        // handle this?
        return;
    }
};

const generateNumbers = () => {
    numbersToSort.value = Array.from({ length: numbersToSortLength.value }, () =>
        Math.floor(Math.random() * numbersToSortLength.value + 1),
    );
    console.log(numbersToSort.value);
};

onMounted(() => {
    generateNumbers();
});

const reloadPage = () => {
    window.location.reload();
}
</script>

<template>
    <div class="d-flex gap-2 align-items-center">
        <h1 class="main-h1">{{ algorithm }} sort</h1>
        <button class="btn btn-outline-primary" @click="reloadPage">
            &lt; choose a different algorithm</button>
    </div>
    <h4 class="text-muted m-4">{{ description }}</h4>
    <div class="algo-visual__wrapper d-flex flex-column gap-4">
        <div class="algo-visual__actions d-flex gap-4 m-4 align-items-center">
            <label class="algo-visual__actions--length">
                Change amount of numbers
                <input class="form-control" type="number" v-model="numbersToSortLength" />
            </label>
            <button class="btn btn-outline-primary" @click="generateNumbers">Generate new numbers</button>
            <button class="btn btn-outline-primary" @click="startSort">Start sort</button>
        </div>
        <AlgoVisualizationBarChart :data="numbersToSort" />
    </div>
</template>

<style scoped lang="scss">
.algo-visual {

    &__actions {
        &--length {
            font-size: 1.2rem;
        }
    }
}
</style>
