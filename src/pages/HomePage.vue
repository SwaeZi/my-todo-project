<!-- 
This file defines the Home View component of a Vue.js application.
It demonstrates how to consume and display data from a Pinia store, and how to call store functions.
-->
<template>
    <main class="container">
        <section class="my-5">
            <h2 class="text-center mb-4">Task List</h2>

            <!-- Displaying Tasks from the Store -->
            <div class="row">
                <div class="col-lg-12">
                    <ul class="list-group">
                        <li v-for="task in tasks" :key="task.id" class="list-group-item mb-3">
                            <div class="d-flex justify-content-between align-items-start">
                                <div>
                                    <h4 class="mb-1">{{ task.title }}</h4>
                                    <p class="mb-1"><strong>Description:</strong> {{ task.description.title }}</p>
                                    <p class="mb-1"><strong>Date to Complete:</strong>
                                        <span class="badge bg-primary">{{ task.description.dateToBeCompleted }}</span>
                                    </p>
                                    <p class="mb-1"><strong>Month to Complete:</strong>
                                        <span class="badge bg-secondary">{{ task.description.monthToBeCompleted
                                            }}</span>
                                    </p>
                                    <p class="mb-1"><strong>Time to Complete:</strong>
                                        <span class="badge bg-info">{{ task.description.timeToBeCompleted }}</span>
                                    </p>
                                    <p class="mb-1"><strong>Priority:</strong>
                                        <span :class="priorityBadgeClass(task.description.priority)" class="badge">{{
                                            task.description.priority }}</span>
                                    </p>
                                    <ul class="list-unstyled mb-2">
                                        <li v-for="(extraInfo, index) in task.description.extraInfoRequired"
                                            :key="index">{{ extraInfo }}</li>
                                    </ul>
                                    <p v-if="task.isCompleted" class="text-success mb-1">Status: Completed</p>
                                    <p v-else class="text-danger mb-1">Status: Incomplete</p>
                                </div>

                                <div class="btn-group">
                                    <button @click="toggleTaskCompletion(task)" class="btn btn-sm"
                                        :class="task.isCompleted ? 'btn-secondary' : 'btn-success'">
                                        {{ task.isCompleted ? 'Undo Completion' : 'Mark as Completed' }}
                                    </button>
                                    <button @click="editTask(task)" class="btn btn-warning btn-sm">Edit Task</button>
                                    <button @click="deleteTask(task.id)" class="btn btn-danger btn-sm">Delete</button>
                                </div>
                            </div>
                        </li>
                    </ul>
                </div>
            </div>
        </section>

        <!-- Personal Note -->
        <section class="mt-5">
            <h3 class="text-center mb-2">A Personal Note</h3>
            <p class="lead text-center">
                Welcome to the Task Management System!
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