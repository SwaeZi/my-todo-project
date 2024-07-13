<template>
    <div class="container">


        <!-- Sign In Form -->
        <form @submit.prevent="signIn" class="w-75 mx-auto">
            <div class="form-group">
                <!-- Email Input -->
                <label class="text-white" for="email">Email address</label>
                <input type="email" class="form-control" id="email" placeholder="Enter email" v-model="formState.email"
                    required />
            </div>

            <div class="form-group">
                <!-- Password Input -->
                <label class="text-white" for="password">Password</label>
                <div class="input-group">
                    <input type="password" class="form-control" id="password" placeholder="Enter password"
                        v-model="formState.password" required />

                </div>
            </div>

            <!-- Sign In Button -->
            <button type="submit" class="btn btn-primary btn-lg btn-block mb-3">Sign In</button>
        </form>
        <!-- End Sign In Form -->

        <!-- Error Message -->
        <p v-if="formState.errorMsg" class="text-danger">{{ formState.errorMsg }}</p>
        <!-- End Error Message -->

        <!-- Sign Up Link -->
        <p class="text-center text-white">
            Don't have an account?
            <PersonalRouter :route="route" :buttonText="buttonText" class="sign-up-link text-primary display-7" />
        </p>
        <!-- End Sign Up Link -->
    </div>
</template>

<script setup>

// ------------------------------------------------------------------------
// Import Block
// ------------------------------------------------------------------------

import { reactive } from "vue";
import { useRouter } from "vue-router";
import PersonalRouter from "./PersonalRouter.vue";
import { useUserStore } from "../stores/user";
import { storeToRefs } from "pinia";

// ------------------------------------------------------------------------
// Variables Block
// ------------------------------------------------------------------------

// Route Variables for navigating users
const route = "/auth/register";
const buttonText = "Sign Up";

// Reactive variable to store email, password, and error messages
const formState = reactive({
    email: "", // Stores the email input
    password: "", // Stores the password input
    errorMsg: "", // Stores any error messages
});

// Router instance for navigation
const router = useRouter();
// Store user accessed easily here
const userStore = useUserStore();
// Destructure the variable 'isLoggedIn' out of the store, keeping its reactivity using the storeToRefs method from Pinia
const { isLoggedIn } = storeToRefs(userStore);

// ------------------------------------------------------------------------
// Functions Block
// ------------------------------------------------------------------------

/**
 * Function to handle the SignIn process.
 * It validates the user's credentials using the signIn function from the user store.
 * On success, it redirects the user to the home page.
 * On failure, it displays an error message.
 */
let signIn = () => {
    try {
        // 1- Hitting the user store, and hitting a function that is used to LOG IN
        userStore.signIn(formState.email, formState.password);
        // 2- Change boolean isLoggedIn variable from the store
        isLoggedIn.value = true;
        // 3 - ReRoute the user to the home page
        router.push({ path: "/" });
    } catch (error) {
        // On failure, let's display an error message
        formState.errorMsg = error.message;
        // Hide this error message after a specific time
        // Hide after 8 seconds, remember that is using always milliseconds
        setTimeout(() => {
            formState.errorMsg = "";
        }, 8000);
    }
};

/*
  The signIn function handles the login process.
  - It validates the user's credentials using the signIn function from the user store.
  - On success, it sets isLoggedIn to true and redirects the user to the home page.
  - On failure, it displays an error message and hides it after 8 seconds.
  */
</script>

<style scoped>
form i {
    margin-left: -30px;
    cursor: pointer;
}

label,
input {
    display: block;
}

button {
    margin: 0.5rem 0;
}
</style>
