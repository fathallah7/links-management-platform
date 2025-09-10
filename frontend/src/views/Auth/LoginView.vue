<script setup lang="ts">
import { RouterLink } from 'vue-router';
import { useRouter } from 'vue-router';
import { ref } from 'vue';
import axios from 'axios';

const router = useRouter();

const email = ref('');
const password = ref('');
const errorMessage = ref('');
const isLoading = ref(false);

const login = async () => {
    isLoading.value = true;
    try {
        const response = await axios.post('/login', {
            email: email.value,
            password: password.value
        });
        console.log(response.data);
        localStorage.setItem('token', response.data.token);
        router.push('/home');
    } catch (error: any) {
        if (error.response && error.response.data && error.response.data.message) {
            errorMessage.value = error.response.data.message;
            console.log(error.response.data.message);
        } else {
            errorMessage.value = 'An error occurred during login.';
        }
        console.error('Login error:', error);
    } finally {
        isLoading.value = false;
    }
};


</script>

<template>
    <div
        class="w-full min-h-screen bg-gradient-to-br from-indigo-100 via-purple-100 to-pink-100 flex items-center justify-center p-4 sm:p-6">
        <div
            class="bg-white rounded-2xl shadow-xl p-8 max-w-lg w-full transform transition-all duration-500 hover:shadow-2xl">
            <!-- Logo -->
            <div class="flex justify-center mb-8">
                <img src="../../assets/logo3.webp" alt="Logo"
                    class="h-20 w-auto transition-transform duration-300 hover:scale-105" />
            </div>

            <!-- Heading -->
            <h2
                class="text-3xl font-bold text-transparent mb-8 text-center bg-clip-text bg-gradient-to-r from-indigo-600 to-purple-600 animate-pulse transition-transform duration-300">
                Welcome Back
            </h2>

            <!-- Form -->
            <form @submit.prevent="login" class="space-y-6">
                <!-- Email Input -->
                <div>
                    <label for="email" class="block text-sm font-medium text-gray-700">Email Address</label>
                    <input v-model="email" type="email" id="email" placeholder="Enter your email"
                        class="mt-1 w-full px-4 py-3 rounded-lg border border-gray-200 bg-gray-50 text-gray-800 placeholder:text-gray-400 focus:ring-2 focus:ring-indigo-500 focus:border-transparent focus:outline-none transition-all duration-300 hover:border-indigo-300"
                        required />
                </div>

                <!-- Password Input -->
                <div>
                    <label for="password" class="block text-sm font-medium text-gray-700">Password</label>
                    <input v-model="password" type="password" id="password" placeholder="Enter your password"
                        class="mt-1 w-full px-4 py-3 rounded-lg border border-gray-200 bg-gray-50 text-gray-800 placeholder:text-gray-400 focus:ring-2 focus:ring-indigo-500 focus:border-transparent focus:outline-none transition-all duration-300 hover:border-indigo-300"
                        required />
                        <div class="flex justify-end">
                            <p>
                                <RouterLink to="/forget-password" class="text-indigo-600 hover:underline text-sm  font-medium">Forgot your password?
                                </RouterLink>
                            </p>
                        </div>
                </div>

                <!-- Error Message -->
                <div v-if="errorMessage" class="text-red-500 text-sm  ">{{ errorMessage }}</div>

                <!-- Submit Button -->
                <div>
                    <button type="submit" :disabled="isLoading"
                        class="w-full px-4 py-3 bg-gradient-to-r from-indigo-600 to-purple-600 text-white rounded-lg hover:from-indigo-700 hover:to-purple-700 focus:ring-2 focus:ring-indigo-500 focus:ring-offset-2 focus:outline-none transition-all duration-300 flex items-center justify-center font-medium"
                        :class="{ 'opacity-50 cursor-not-allowed': isLoading }">
                        <span>{{ isLoading ? 'Signing In...' : 'Sign In' }}</span>
                    </button>
                </div>

                <!-- Google Sign-In Button -->
                <div>
                    <button @click="" type="button"
                        class="w-full px-4 py-3 bg-white border border-gray-200 rounded-lg hover:bg-gray-100 cursor-pointer focus:ring-2 focus:ring-indigo-500 focus:outline-none transition-all duration-300 flex items-center justify-center space-x-2">
                        <svg class="w-5 h-5" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
                            <path
                                d="M22.56 12.25c0-.78-.07-1.53-.2-2.25H12v4.26h5.92c-.26 1.37-1.04 2.53-2.21 3.31v2.77h3.57c2.08-1.92 3.28-4.74 3.28-8.09z"
                                fill="#4285F4" />
                            <path
                                d="M12 23c2.97 0 5.46-.98 7.28-2.66l-3.57-2.77c-1.02.68-2.33 1.09-3.71 1.09-2.86 0-5.29-1.93-6.16-4.53H2.18v2.84C4.01 20.07 7.56 23 12 23z"
                                fill="#34A853" />
                            <path
                                d="M5.84 14.09c-.22-.66-.35-1.36-.35-2.09s.13-1.43.35-2.09V7.07H2.18C1.44 8.45 1 10.18 1 12s.44 3.55 1.18 4.93l3.66-2.84z"
                                fill="#FBBC05" />
                            <path
                                d="M12 5.38c1.62 0 3.06.56 4.21 1.64l3.15-3.15C17.45 2.09 14.97 1 12 1 7.56 1 4.01 3.93 2.18 7.07l3.66 2.84c.87-2.6 3.3-4.53 6.16-4.53z"
                                fill="#EA4335" />
                        </svg>
                        <span class="text-gray-700 font-medium">Sign in with Google</span>
                    </button>
                </div>

                <!-- Additional Links -->
                <div class="text-center text-sm text-gray-600 space-y-3">
                    <p>
                        Don't have an account?
                        <RouterLink to="/register" class="text-indigo-600 hover:underline font-medium">Sign up
                        </RouterLink>
                    </p>
                </div>
            </form>

        </div>
    </div>
</template>
