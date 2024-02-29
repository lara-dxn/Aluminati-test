/**
	general changes: types added where and as appropriate to prevent e.g. implicit return types of any
	adjusted function names somewhat to make their roles clearer


*/

<script setup lang="ts">
import { Ref, ref, watch, onMounted } from 'vue';

const numbers: Ref<number[]> = ref([]);
const limit: Ref<number> = ref(100);

function generateArray(): void {
    numbers.value.length = 0;
    for (let i = 1; i <= limit.value; i++) {
        numbers.value.push(i);
    }
    numbers.value.sort(() => Math.random() - 0.5);

}
function hoverHighlight(number: number): void {
    const nums: NodeListOf<Element> = document.querySelectorAll('.number');
    nums.forEach(numElement => {
        const num: number = parseInt(numElement.textContent!.trim());
        if (number !== num && number % num === 0) {
            numElement.classList.add('active');
        }
    });
}

function resetHighlight(): void {
    const nums: NodeListOf<Element> = document.querySelectorAll('.number');
    nums.forEach(numElement => numElement.classList.remove('active'));
}

onMounted(() => {
    generateArray();
});

//if limit changes, create new array of correct length, unless length<0, which would not make sense here
watch(limit, (newLimit: number) => {
    if (newLimit < 0) {
        limit.value = 0;
    }
    generateArray();
});
</script>

<template>
    <div>
        <input type="number" v-model="limit" /><br /><br />
        <div class="number"
            v-for="number in numbers"
            :key="number"
            @mouseover="hoverHighlight(number)"
            @mouseout="resetHighlight"
        >
            {{ number }}
        </div>
    </div>
</template>

<style>
.number {
    display: inline-block;
    padding: 5px;
    background-color: lightgrey;
    margin: 5px;
}

.active {
    background-color: red;
}
</style>
