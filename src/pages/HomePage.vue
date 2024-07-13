<!-- 
This file defines the Home View component of a Vue.js application.
It demonstrates how to consume and display data from a Pinia store, and how to call store functions.
-->
<template>
    <main class="container">
        <section class="my-5">
            <h2 class="text-center mb-4">Welcome to the Home View</h2>

            <!-- Displaying Data from the Store -->
            <div class="text-center">
                <h4>Current Count: {{ countNumber }}</h4>
                <h5>Doubled Count: {{ doubleCountNumber }}</h5>
            </div>

            <!-- Button to Increment Count -->
            <div class="text-center mt-4">
                <button @click="incrementNumberFunctionFromStore" class="btn btn-primary">Add +1</button>
            </div>
        </section>

        <!-- Personal Note -->
        <section class="mt-5">
            <h3 class="text-center mb-2">A Personal Note</h3>
            <p class="lead text-center">
                Welcome to Home View!
            </p>
        </section>
    </main>
</template>

<script setup>
// ------------------------------------------------------------------------
// Import Block
// ------------------------------------------------------------------------

// Import computed from Vue for reactive calculations
import { computed } from "vue";
// Import useCounterStore to access the counter store
import { useCounterStore } from "../stores/counter";
import { useUserStore } from "../stores/user";
import { storeToRefs } from "pinia";

// ------------------------------------------------------------------------
// Store Access Block
// ------------------------------------------------------------------------

// Access the counter store and store it in a variable for easier reference
let countStore = useCounterStore();

// ------------------------------------------------------------------------
// Computed Properties Block
// ------------------------------------------------------------------------

// Store the 'count' variable from the store in a computed property for real-time updates
let countNumber = computed(() => countStore.count);

// Store the 'doubleCount' variable from the store
let doubleCountNumber = countStore.doubleCount;

// ------------------------------------------------------------------------
// Methods Block
// ------------------------------------------------------------------------

// Access the 'increment' function from the store to be used in the template
let incrementNumberFunctionFromStore = countStore.increment;

const userStore = useUserStore();
const { getTasksForUser } = storeToRefs(userStore);

console.log(userStore.getTasksForUser());

/*
  The incrementNumberFunctionFromStore is used to increment the count in the store.
  - It accesses the increment function from the counter store.
  - This function is called when the button in the template is clicked.
  */
</script>

<style scoped>
h2,h3,h4,h5,p {
    color: white;
}


</style>