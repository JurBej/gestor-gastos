<script setup>
import {ref} from 'vue';
const transactions = ref ([
    {id: 1, text: 'Flower', amount: -19.99},
    {id: 2, text: 'Salary', amount: 219.99},
    {id: 3, text: 'Book', amount: -109.99},
    {id: 4, text: 'Camera', amount: 419.99},
])
</script>

<template>
    <h3>History</h3>
    <ul id="list" class="list">
        <li v-for="(transaction) in transactions" :key="transaction.id" :class="transaction.amount<0 ? 'minus' : 'plus'">
            {{ transaction.text }} <span>{{ transaction.amount }}</span><button class="delete-btn">x</button>
        </li>
        <!-- <li class="plus">
            Cash <span>+$200</span><button class="delete-btn">x</button>
        </li> -->
    </ul>
</template>

<style scoped></style>