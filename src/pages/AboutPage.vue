<template>
    <main class="container">
        <section class="my-5">
            <h2 class="text-center text-white mb-4">About Us</h2>

            <div>
                <p class="text-white">
                    Lorem ipsum dolor sit amet, consectetur adipiscing elit. Sed aliquet augue vel urna sodales, vel
                    tempor diam
                    faucibus. Quisque ut consequat magna. Sed eu sapien velit. Proin dignissim leo a ante accumsan, sit
                    amet
                    laoreet augue bibendum. Nunc condimentum sit amet nulla at hendrerit. Nulla scelerisque ligula a
                    felis
                    pretium eleifend.
                </p>
                <p class="text-white">
                    Phasellus ultricies semper urna, vitae varius orci dapibus vel. Nullam sagittis dolor ac tincidunt
                    ullamcorper. Fusce eget fringilla sem, sit amet fermentum felis. Vestibulum sit amet malesuada
                    lorem. Donec
                    placerat diam in erat cursus, nec hendrerit nulla mattis.
                </p>
            </div>

            <!-- Button to Trigger Action -->
            <div class="text-center mt-4">
                <button @click="incrementNumberFunctionFromStore" class="btn btn-primary">Add +1</button>
            </div>
        </section>

        <!-- Professional Tip -->
        <section class="mt-5">
            <h3 class="text-center text-white mb-4 ">Professional Tip</h3>
            <p class=" lead">

                </p>
        </section>
    </main>
</template>

<script setup>
import { computed } from "vue";
import { useCounterStore } from "../stores/counter";
// 2nd wa
// A bit easier because we can store the access of the store inside a variable which we then reference easier (take a look at the example with the vaar name 'countNumber')
let countStore = useCounterStore();

// -----
// -----
// Consuming a piece of state that happens to be a function
// How do we do it ?
let incrementNumberFunctionFromStore = countStore.increment;
</script>
