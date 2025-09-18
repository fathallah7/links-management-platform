<script setup>
import { RouterLink } from 'vue-router';
import { useRouter } from 'vue-router';
import { ref, onMounted } from 'vue';
import axios from 'axios';
import GoogleLogin from './components/GoogleLogin.vue';

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
    } catch (error) {
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
                            <RouterLink to="/forget-password"
                                class="text-indigo-600 hover:underline text-sm  font-medium">Forgot your password?
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

                <GoogleLogin />

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
