<script setup>
import { RouterLink } from 'vue-router';
import { ref } from 'vue';
import axios from 'axios';
import { useToast } from 'vue-toastification';

const isLoading = ref(false);
const toast = useToast();
const email = ref('');
const errorMessage = ref('');

const sendResetLink = async () => {
    try {
        isLoading.value = true;
        errorMessage.value = '';
        const response = await axios.post('/forgot-password', { email: email.value });
        toast.success('Reset link sent!');
        console.log(response.data);
        email.value = '';
        isLoading.value = false;
    } catch (error) {
        isLoading.value = false;
        if (error.response && error.response.data) {
            errorMessage.value = error.response.data.message || 'Unknown error';
            // toast.error(errorMessage.value);
        } else {
            console.error(error);
            toast.error('Error sending reset link');
        }
    }
};

</script>


<template>
    <div
        class="w-full min-h-screen bg-gradient-to-br from-indigo-100 via-purple-100 to-pink-100 flex items-center justify-center p-4 sm:p-6">
        <div
            class="bg-white rounded-2xl shadow-xl p-6 max-w-lg w-full transform transition-all duration-500 hover:shadow-2xl">
            <!-- Logo -->
            <div class="flex justify-center mb-6">
                <img src="../../assets/logo3.webp" alt="Logo"
                    class="h-16 w-auto transition-transform duration-300 hover:scale-105" />
            </div>

            <!-- Heading -->
            <h2
                class="text-2xl font-bold text-transparent mb-6 text-center bg-clip-text bg-gradient-to-r from-indigo-600 to-purple-600 animate-pulse">
                Reset Password
            </h2>

            <!-- Form -->
            <form @submit.prevent="sendResetLink" class="space-y-4">
                <!-- Email Input -->
                <div>
                    <label for="email" class="block text-sm font-medium text-gray-700">Email Address</label>
                    <input v-model="email" type="email" id="email" placeholder="Enter your email"
                        class="mt-1 w-full px-4 py-2.5 rounded-lg border border-gray-200 bg-gray-50 text-gray-800 placeholder:text-gray-400 focus:ring-2 focus:ring-indigo-500 focus:border-transparent focus:outline-none transition-all duration-300 hover:border-indigo-300"
                        :class="{ 'border-red-500': errorMessage }" required />
                </div>
                <!-- Error Message -->
                <div v-if="errorMessage" class="text-red-500 text-sm mt-1 ">{{ errorMessage }}</div>

                <!-- Submit Button -->
                <div>
                    <button type="submit" :disabled="isLoading" :class="{ 'opacity-50 cursor-not-allowed': isLoading }"
                        class="w-full px-4 py-2.5 bg-gradient-to-r from-indigo-600 to-purple-600 text-white rounded-lg hover:from-indigo-700 hover:to-purple-700 focus:ring-2 focus:ring-indigo-500 focus:ring-offset-2 focus:outline-none transition-all duration-300 flex items-center justify-center font-medium">
                        Send Reset Link
                    </button>
                </div>

                <!-- Back to Login Link -->
                <div class="text-center text-sm text-gray-600">
                    <p>
                        Back to
                        <RouterLink to="/login" class="text-indigo-600 hover:underline font-medium">Login</RouterLink>
                    </p>
                </div>
            </form>
        </div>
    </div>
</template>
