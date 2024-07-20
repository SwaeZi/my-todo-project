<!--
This file defines a Vue.js component for displaying and managing tasks in a to-do application.
By building this component, we will achieve a user interface that shows a list of all tasks,
allowing users to mark tasks as completed and delete them, leveraging global state management with Pinia.js.
-->
<template>
    <div class="container">
        <h2 class="text-center text-white mb-4">Task List</h2>

        <ul class="list-group">
            <li v-for="task in tasks" :key="task.id" class="list-group-item mb-3">
                <div v-if="!task.isEditing">
                    <div class="d-flex justify-content-between align-items-start">
                        <div>
                            <h4 class="mb-1">{{ task.title }}</h4>
                            <p class="mb-1"><strong>Description:</strong> {{ task.description.title }}</p>
                            <p class="mb-1"><strong>Date to Complete:</strong>
                                <span class="badge bg-primary">{{ task.description.dateToBeCompleted }}</span>
                            </p>
                            <p class="mb-1"><strong>Month to Complete:</strong>
                                <span class="badge bg-secondary">{{ task.description.monthToBeCompleted }}</span>
                            </p>
                            <p class="mb-1"><strong>Time to Complete:</strong>
                                <span class="badge bg-info">{{ task.description.timeToBeCompleted }}</span>
                            </p>
                            <p class="mb-1"><strong>Priority:</strong>
                                <span :class="priorityBadgeClass(task.description.priority)" class="badge">{{
                                    task.description.priority }}</span>
                            </p>
                            <ul class="list-unstyled mb-2">
                                <li v-for="(extraInfo, index) in task.description.extraInfoRequired" :key="index">{{
                                    extraInfo }}</li>
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
                </div>

                <!-- Editable Form -->
                <div v-else>
                    <form @submit.prevent="updateTask(task)">
                        <div class="mb-3">
                            <label for="editTitle-{{ task.id }}" class="form-label">Title:</label>
                            <input v-model="task.title" type="text" :id="'editTitle-' + task.id" class="form-control"
                                required />
                        </div>

                        <div class="mb-3">
                            <label for="editDescriptionTitle-{{ task.id }}" class="form-label">Description
                                Title:</label>
                            <input v-model="task.description.title" type="text" :id="'editDescriptionTitle-' + task.id"
                                class="form-control" required />
                        </div>

                        <div class="mb-3">
                            <label for="editDateToBeCompleted-{{ task.id }}" class="form-label">Date to be
                                Completed:</label>
                            <input v-model="task.description.dateToBeCompleted" type="date"
                                :id="'editDateToBeCompleted-' + task.id" class="form-control" required />
                        </div>

                        <div class="mb-3">
                            <label for="editMonthToBeCompleted-{{ task.id }}" class="form-label">Month to be
                                Completed:</label>
                            <input v-model="task.description.monthToBeCompleted" type="month"
                                :id="'editMonthToBeCompleted-' + task.id" class="form-control" required />
                        </div>

                        <div class="mb-3">
                            <label for="editTimeToBeCompleted-{{ task.id }}" class="form-label">Time to be
                                Completed:</label>
                            <input v-model="task.description.timeToBeCompleted" type="time"
                                :id="'editTimeToBeCompleted-' + task.id" class="form-control" required />
                        </div>

                        <div class="mb-3">
                            <label for="editPriority-{{ task.id }}" class="form-label">Priority:</label>
                            <select v-model="task.description.priority" :id="'editPriority-' + task.id"
                                class="form-select" required>
                                <option value="" disabled>Select priority</option>
                                <option value="light">Light</option>
                                <option value="moderate">Moderate</option>
                                <option value="high">High</option>
                            </select>
                        </div>

                        <div class="mb-3">
                            <label for="editExtraInfo-{{ task.id }}" class="form-label">Extra Info Required:</label>
                            <div class="input-group">
                                <input v-model="newExtraInfo" type="text" :id="'editExtraInfo-' + task.id"
                                    class="form-control" />
                                <button type="button" class="btn btn-outline-secondary" @click="addExtraInfo(task)">Add
                                    Info</button>
                            </div>
                            <ul class="list-unstyled mt-2">
                                <li v-for="(info, index) in task.description.extraInfoRequired" :key="index">{{ info
                                    }}<button type="button" class="btn btn-sm btn-danger ms-2"
                                        @click="removeExtraInfo(task, index)">Remove</button></li>
                            </ul>
                        </div>

                        <button type="submit" class="btn btn-primary">Update Task</button>
                        <button type="button" @click="cancelEdit(task)" class="btn btn-secondary ms-2">Cancel</button>
                    </form>
                </div>
            </li>
        </ul>

        <div v-if="tasks.length === 0" class="text-center mt-3">
            <p>No tasks found.</p>
        </div>
    </div>
</template>


<script setup>
// ------------------------------------------------------------------------
// Import Block
// ------------------------------------------------------------------------

// Import the useTaskStore function from taskStore to interact with the task store
import { useTaskStore } from "../stores/taskStore";

// ------------------------------------------------------------------------
// Store Access Block
// ------------------------------------------------------------------------

// Use the task store by saving it in a variable
const taskstore = useTaskStore();

// Destructure all the possible pieces of data that we want out of this
const { tasks, deleteTask, markTaskCompleted } = taskstore; // Destructure necessary functions and state from the task store

const priorityBadgeClass = (priority) => {
    switch (priority) {
        case 'light':
            return 'bg-light text-dark';
        case 'moderate':
            return 'bg-warning text-dark';
        case 'high':
            return 'bg-danger text-white';
        default:
            return 'bg-secondary text-white';
    }
};

/*
  The useTaskStore function is used to access the task store.
  - Destructure tasks, markTaskCompleted, and deleteTask from the task store.
  - These will be used to interact with the global state of tasks.
  */

const toggleTaskCompletion = (task) => {
    task.isCompleted = !task.isCompleted;
    // Normally you would also update the task in the store
    taskStore.updateTask(task); // Assuming there's an updateTask method in the store
};

// Edit task function
const editTask = (task) => {
    task.isEditing = true;
};

// Update task function
const updateTask = (task) => {
    task.isEditing = false;
    // You would normally handle the update logic here
    taskStore.updateTask(task); // Assuming there's an updateTask method in the store
};

// Cancel edit function
const cancelEdit = (task) => {
    task.isEditing = false;
    // Reset to original task state if necessary
    taskStore.resetTask(task); // Assuming there's a resetTask method in the store
};

// Add extra info function
const addExtraInfo = (task) => {
    if (newExtraInfo) {
        task.description.extraInfoRequired.push(newExtraInfo);
        newExtraInfo = '';
    }
};

// Remove extra info function
const removeExtraInfo = (task, index) => {
    task.description.extraInfoRequired.splice(index, 1);
};



</script>

<style scoped>
.container {
    max-width: 800px;
    margin: 0 auto;
}

.btn-group button {
    margin-right: 10px;
}
</style>

<!--
Summary:
This file implements a Vue.js component that displays a list of tasks from the global state managed by Pinia.js.
It allows users to mark tasks as completed or delete them. The component leverages Pinia's state management to
interact with the tasks and provide necessary functionalities.
-->
