<script setup>
import { ref } from 'vue';
import axios from 'axios';
import { useRouter } from 'vue-router';
import { useToast } from 'vue-toastification';

const toast = useToast();
const router = useRouter();
const errorMessages = ref({});
const isLoading = ref(false)

const newUserData = ref({
    name: '',
    email: '',
    password: '',
    password_confirmation: ''
});

const register = async () => {
    try {
        errorMessages.value = '';
        isLoading.value = true;
        const response = await axios.post('/register', newUserData.value);
        console.log(response.data)
        router.push('/login');
        toast.success('Registration successful! Please log in.');
    } catch (error) {
        isLoading.value = false;
        if (error.response && error.response.data && error.response.data.errors) {
            errorMessages.value = error.response.data.errors;
            console.log(error.response.data.message);
        } else {
            errorMessage.value = 'An error occurred during login.';
        }
        console.error('Login error:', error);
    } finally {
        isLoading = false;
    }
}



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
                Create Account
            </h2>

            <!-- Form -->
            <form @submit.prevent="register" class="space-y-4">
                <!-- Name Input -->
                <div>
                    <label for="name" class="block text-sm font-medium text-gray-700">Name</label>
                    <input v-model="newUserData.name" type="text" id="name" placeholder="Enter your Name"
                        class="mt-1 w-full px-4 py-2.5 rounded-lg border border-gray-200 bg-gray-50 text-gray-800 placeholder:text-gray-400 focus:ring-2 focus:ring-indigo-500 focus:border-transparent focus:outline-none transition-all duration-300 hover:border-indigo-300"
                        :class="{ 'border-red-700': errorMessages.name }" required />
                    <!-- Error Message -->
                    <div v-if="errorMessages.name" class="text-red-500 text-sm">
                        {{ errorMessages.name[0] }}
                    </div>
                </div>

                <!-- Email Input -->
                <div>
                    <label for="email" class="block text-sm font-medium text-gray-700">Email Address</label>
                    <input v-model="newUserData.email" type="email" id="email" placeholder="Enter your email"
                        class="mt-1 w-full px-4 py-2.5 rounded-lg border border-gray-200 bg-gray-50 text-gray-800 placeholder:text-gray-400 focus:ring-2 focus:ring-indigo-500 focus:border-transparent focus:outline-none transition-all duration-300 hover:border-indigo-300"
                        :class="{ 'border-red-700': errorMessages.email }" required />
                    <!-- Error Message -->
                    <div v-if="errorMessages.email" class="text-red-500 text-sm">
                        {{ errorMessages.email[0] }}
                    </div>
                </div>

                <!-- Password Input -->
                <div>
                    <label for="password" class="block text-sm font-medium text-gray-700">Password</label>
                    <input v-model="newUserData.password" type="password" id="password"
                        placeholder="Enter your password"
                        class="mt-1 w-full px-4 py-2.5 rounded-lg border border-gray-200 bg-gray-50 text-gray-800 placeholder:text-gray-400 focus:ring-2 focus:ring-indigo-500 focus:border-transparent focus:outline-none transition-all duration-300 hover:border-indigo-300"
                        :class="{ 'border-red-700': errorMessages.password }" required />
                    <!-- Error Message -->
                    <div v-if="errorMessages.password" class="text-red-500 text-sm">
                        {{ errorMessages.password[0] }}
                    </div>
                </div>

                <!-- Confirm Password Input -->
                <div>
                    <label for="confirmPassword" class="block text-sm font-medium text-gray-700">Confirm
                        Password</label>
                    <input v-model="newUserData.password_confirmation" type="password" id="confirmPassword"
                        placeholder="Confirm your password"
                        class="mt-1 w-full px-4 py-2.5 rounded-lg border border-gray-200 bg-gray-50 text-gray-800 placeholder:text-gray-400 focus:ring-2 focus:ring-indigo-500 focus:border-transparent focus:outline-none transition-all duration-300 hover:border-indigo-300"
                        :class="{ 'border-red-700': errorMessages.password_confirmation }" required />
                    <!-- Error Message -->
                    <div v-if="errorMessages.password_confirmation" class="text-red-500 text-sm">
                        {{ errorMessages.password_confirmation[0] }}
                    </div>
                </div>


                <!-- Submit Button -->
                <div>
                    <button type="submit" :disabled="isLoading"
                        class="w-full px-4 py-3 bg-gradient-to-r from-indigo-600 to-purple-600 text-white rounded-lg hover:from-indigo-700 hover:to-purple-700 focus:ring-2 focus:ring-indigo-500 focus:ring-offset-2 focus:outline-none transition-all duration-300 flex items-center justify-center font-medium"
                        :class="{ 'opacity-50 cursor-not-allowed': isLoading }">
                        <span>{{ isLoading ? 'Signing Up...' : 'Register' }}</span>
                    </button>
                </div>

                <!-- Google Sign-In Button -->
                <div>
                    <button @click="googleSignIn" type="button"
                        class="w-full px-4 py-2.5 bg-white border border-gray-200 rounded-lg hover:bg-gray-100 cursor-pointer focus:ring-2 focus:ring-indigo-500 focus:outline-none transition-all duration-300 flex items-center justify-center space-x-2">
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
                        <span class="text-gray-700 font-medium">Sign up with Google</span>
                    </button>
                </div>

                <!-- Additional Links -->
                <div class="text-center text-sm text-gray-600 space-y-3">
                    <p>
                        Already have an account?
                        <RouterLink to="/login" class="text-indigo-600 hover:underline font-medium">Sign in</RouterLink>
                    </p>
                </div>
            </form>
        </div>
    </div>
</template>