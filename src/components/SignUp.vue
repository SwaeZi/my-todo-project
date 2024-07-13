<template>
    <div class="container">
        <!-- Header Section -->
        <div class="header text-center py-5">
            <div class="header-description">
                <h3 class="header-title display-4 fw-bold text-primary">Register to <br><span
                        class="text-secondary">Start
                        organizing your tasks!</span></h3>
            </div>
        </div>

        <!-- Sign Up Form -->
        <form @submit.prevent="signUp" class="form-sign-up mt-4">
            <div class="form">
                <!-- Email Input Field -->
                <div class="form-group">
                    <label for="email" class="input-field-label text-white">Your Email</label>
                    <input type="email" class="form-control" id="email" placeholder="Enter your email"
                        v-model="formState.email" required />
                </div>

                <!-- Password Input Field -->
                <div class="form-group">
                    <label for="password" class="input-field-label text-white">Your Password</label>
                    <input type="password" class="form-control" id="password" placeholder="Enter your password"
                        v-model="formState.password" required />
                </div>

                <!-- Confirm Password Input Field -->
                <div class="form-group">
                    <label for="confirmPassword" class="input-field-label text-white">Confirm Password</label>
                    <input type="password" class="form-control" id="confirmPassword" placeholder="Confirm your password"
                        v-model="formState.confirmPassword" required />
                </div>

                <!-- Sign Up Button with Margin -->
                <button type="submit" class="btn btn-primary btn-block mt-3">Sign Up</button>

                <!-- Have an account? Sign In link -->
                <p class="mt-3 text-center text-white">
                    Already have an account?
                    <PersonalRouter :route="goToRoute" :buttonText="buttonText" class="btn btn-link" />
                </p>
            </div>
        </form>

        <!-- Error Message Display -->
        <div v-if="formState.errorMsg" class="alert alert-danger mt-3">
            {{ formState.errorMsg }}
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            formState: {
                email: '',
                password: '',
                confirmPassword: '',
                errorMsg: ''
            }
        };
    },
    methods: {
        signUp() {
            // Implement your sign up logic here
            console.log('Signing up...');
        }
    }
};
</script>

<script setup>
// ------------------------------------------------------------------------
// Import Block
// ------------------------------------------------------------------------

// Importing reactive from Vue to create a reactive form state object
import { reactive } from "vue";
// Importing useRouter from vue-router for navigation
import { useRouter } from "vue-router";
// Importing PersonalRouter component for navigation links
import PersonalRouter from "./PersonalRouter.vue";
// Importing the useUserStore function from userStore to interact with the user store
import { useUserStore } from "../stores/user";

// ------------------------------------------------------------------------
// Variables Block
// ------------------------------------------------------------------------

// Route Variables for navigating users
// Remember they are just storing strings
// Not reactive in nature, so nor eff or reactive
const goToRoute = "/auth/login";
const buttonText = "Sign In";

// Consolidating input fields and error messages into a reactive object
const formState = reactive({
    email: "", // Stores the email input
    password: "", // Stores the password input
    confirmPassword: "", // Stores the confirm password input
    errorMsg: "", // Stores any error messages
});

// Router instance for navigation
const router = useRouter();
// Store user accessed easily here
const userStore = useUserStore();

// ------------------------------------------------------------------------
// Functions Block
// ------------------------------------------------------------------------

// Function to handle the SignUp process
const signUp = () => {
    // Conditional Logic only using a simple IF statement
    if (formState.password === formState.confirmPassword) {
        try {
            // Utilizes the register function from the user store to register the user
            userStore.register(formState.email, formState.password);
            // On successful sign up, redirect the user to the login page
            router.push({ path: goToRoute });
        } catch (error) {
            // On failure, display an error message
            formState.errorMsg = error.message;
            // Automatically clear the error message after 5 seconds
            setTimeout(() => {
                formState.errorMsg = "";
            }, 5000);
        }
        return;
    }
    // Sets error message if passwords do not match
    formState.errorMsg = "Passwords do not match. Please try again.";
    setTimeout(() => {
        formState.errorMsg = "";
    }, 2000);
};

/*
  The signUp function handles the registration process.
  - It checks if the passwords match and, if so, calls the register function from the user store to register the user.
  - On successful registration, it redirects the user to the login page.
  - If there is an error during registration, it displays an error message and clears it after 5 seconds.
  - If the passwords do not match, it sets an error message and clears it after 2 seconds.
  */
</script>

<style scoped>
.input-field-label {
    font-weight: bold;
}
</style>