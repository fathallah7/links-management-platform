<script setup lang="ts">
import { ref, computed } from 'vue';
import { useRoute, RouterLink, useRouter } from 'vue-router';
import axios from 'axios';
import { useToast } from 'vue-toastification';

const toast = useToast();
const route = useRoute();
const router = useRouter();
const isLoading = ref(false);

const password = ref('');
const confirmPassword = ref('');
const errorMessage = ref('');
const successMessage = ref('');


const token = route.params.token as string;
const email = route.query.email as string;


const passwordError = computed(() => {
    if (!password.value) return 'Password is required';
    if (password.value.length < 8) return 'Password must be at least 8 characters';
    return '';
});

const confirmPasswordError = computed(() => {
    if (!confirmPassword.value) return 'Password confirmation is required';
    if (confirmPassword.value !== password.value) return 'Passwords do not match the password entered above';
    return '';
});

const isFormValid = computed(() => !passwordError.value && !confirmPasswordError.value);


const handleSubmit = async () => {

    if (!isFormValid.value) {
        toast.error('Please fix the errors in the form');
        return;
    }

    isLoading.value = true;

    try {
        errorMessage.value = '';
        successMessage.value = '';
        const response = await axios.post('/reset-password', {
            email,
            token,
            password: password.value,
            password_confirmation: confirmPassword.value,
        });
        toast.success('Password has been reset successfully!');
        password.value = '';
        confirmPassword.value = '';
        router.push('/login');
    }
    catch (error) {
        if (error.response && error.response.data) {
            errorMessage.value = error.response.data.message || 'Unknown error';
            toast.error(errorMessage.value);
        } else {
            toast.error('Error resetting password');
        }
        console.error(error);
    } finally {
        isLoading.value = false;
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
                Set New Password
            </h2>

            <!-- Form -->
            <form @submit.prevent="handleSubmit" class="space-y-4">
                <!-- Password Input -->
                <div>
                    <label for="password" class="block text-sm font-medium text-gray-700">New Password</label>
                    <input v-model="password" type="password" id="password" placeholder="Enter new password"
                        class="mt-1 w-full px-4 py-2.5 rounded-lg border border-gray-200 bg-gray-50 text-gray-800 placeholder:text-gray-400 focus:ring-2 focus:ring-indigo-500 focus:border-transparent focus:outline-none transition-all duration-300 hover:border-indigo-300"
                        :class="{ 'border-red-500': passwordError }" required />
                    <p v-if="passwordError" class="mt-1 text-sm text-red-500">{{ passwordError }}</p>
                </div>

                <!-- Confirm Password Input -->
                <div>
                    <label for="confirm-password" class="block text-sm font-medium text-gray-700">Confirm
                        Password</label>
                    <input v-model="confirmPassword" type="password" id="confirm-password"
                        placeholder="Confirm password"
                        class="mt-1 w-full px-4 py-2.5 rounded-lg border border-gray-200 bg-gray-50 text-gray-800 placeholder:text-gray-400 focus:ring-2 focus:ring-indigo-500 focus:border-transparent focus:outline-none transition-all duration-300 hover:border-indigo-300"
                        :class="{ 'border-red-500': confirmPasswordError }" required />
                    <p v-if="confirmPasswordError" class="mt-1 text-sm text-red-500">{{ confirmPasswordError }}</p>
                </div>

                <!-- Error Message -->
                <div v-if="errorMessage" class="text-red-500 text-sm mt-2">{{ errorMessage }}</div>
                <!-- Success Message -->
                <div v-if="successMessage" class="text-green-500 text-sm mt-2">{{ successMessage }}</div>

                <!-- Submit Button -->
                <div>
                    <button type="submit" :disabled="isLoading || !isFormValid"
                        :class="{ 'opacity-50 cursor-not-allowed': isLoading || !isFormValid }"
                        class="w-full px-4 py-2.5 bg-gradient-to-r from-indigo-600 to-purple-600 text-white rounded-lg hover:from-indigo-700 hover:to-purple-700 focus:ring-2 focus:ring-indigo-500 focus:ring-offset-2 focus:outline-none transition-all duration-300 flex items-center justify-center font-medium">
                        <svg v-if="isLoading" class="animate-spin h-5 w-5 mr-2 text-white"
                            xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24">
                            <circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4">
                            </circle>
                            <path class="opacity-75" fill="currentColor"
                                d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z">
                            </path>
                        </svg>
                        <span>{{ isLoading ? 'Saving...' : 'Reset Password' }}</span>
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
