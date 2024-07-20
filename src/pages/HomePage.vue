<template>
    <main class="container my-5">
        <section>
            <h2 class="text-center mb-4"> Welcome to the Task Management System!</h2>

            <!-- Displaying Tasks from the Store -->
            <div class="row">
                <div class="col-lg-12">
                    <div class="card shadow-sm">
                        <div class="card-body">
                            <ul class="list-group">
                                <li v-for="task in tasks" :key="task.id"
                                    class="list-group-item d-flex justify-content-between align-items-start mb-3">
                                    <div class="me-3">
                                        <h4 class="mb-1">{{ task.title }}</h4>
                                        <p class="mb-1"><strong>Description:</strong> {{ task.description.title }}</p>
                                        <p class="mb-1"><strong>Date to Complete:</strong>
                                            <span class="badge bg-primary">{{ task.description.dateToBeCompleted
                                                }}</span>
                                        </p>
                                        <p class="mb-1"><strong>Month to Complete:</strong>
                                            <span class="badge bg-secondary">{{ task.description.monthToBeCompleted
                                                }}</span>
                                        </p>
                                        <p class="mb-1"><strong>Time to Complete:</strong>
                                            <span class="badge bg-info">{{ task.description.timeToBeCompleted }}</span>
                                        </p>
                                        <p class="mb-1"><strong>Priority:</strong>
                                            <span :class="priorityBadgeClass(task.description.priority)"
                                                class="badge">{{ task.description.priority }}</span>
                                        </p>
                                        <ul class="list-unstyled mb-2">
                                            <li v-for="(extraInfo, index) in task.description.extraInfoRequired"
                                                :key="index">{{ extraInfo }}</li>
                                        </ul>
                                        <p v-if="task.isCompleted" class="text-success mb-1"><i
                                                class="bi bi-check-circle"></i> Status: Completed</p>
                                        <p v-else class="text-danger mb-1"><i class="bi bi-x-circle"></i> Status:
                                            Incomplete</p>
                                    </div>

                                    <div class="btn-group">
                                        <button @click="toggleTaskCompletion(task)" class="btn btn-sm"
                                            :class="task.isCompleted ? 'btn-secondary' : 'btn-success'">
                                            {{ task.isCompleted ? 'Undo Completion' : 'Mark as Completed' }}
                                        </button>
                                        <button @click="editTask(task)" class="btn btn-warning btn-sm ms-2">Edit
                                            Task</button>
                                        <button @click="deleteTask(task.id)"
                                            class="btn btn-danger btn-sm ms-2">Delete</button>
                                    </div>
                                </li>
                            </ul>
                        </div>
                    </div>
                </div>
            </div>
        </section>

    </main>
</template>

<script setup>
// Import necessary functions and hooks from Vue and Pinia
import { computed } from 'vue';
import { useTaskStore } from '../stores/taskStore'; // Adjust the import path as necessary

// Initialize the task store
const taskStore = useTaskStore();

// Computed property to access tasks
const tasks = computed(() => taskStore.tasks);

// Method to toggle task completion
const toggleTaskCompletion = (task) => {
    task.isCompleted = !task.isCompleted;
    taskStore.updateTask(task); // Ensure there's an updateTask method in the task store
};

// Method to edit a task
const editTask = (task) => {
    // Implement the logic for editing a task
};

// Method to delete a task
const deleteTask = (taskId) => {
    taskStore.deleteTask(taskId); // Ensure there's a deleteTask method in the task store
};

// Method to return the appropriate Bootstrap class for the priority badge
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
</script>

<style scoped>

h2 {
    color: white;
}

h3,
h4,
h5,
p {
    color: #575757;
    /* Darker text color for better readability */
}

.card {
    border-radius: 10px;
}

.card-body {
    padding: 2rem;
}
</style>
