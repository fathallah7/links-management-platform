<script setup>
import { ref } from 'vue';
import axios from 'axios';
import { useRouter } from 'vue-router';
import { useToast } from 'vue-toastification';
import GoogleLogin from './components/GoogleLogin.vue';

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
                <GoogleLogin />

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