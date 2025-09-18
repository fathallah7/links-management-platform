<script setup lang="ts">
import { ref } from 'vue';

// Reactive state for sidebar visibility
const isSidebarOpen = ref(true);

// Function to toggle sidebar
const toggleSidebar = () => {
    isSidebarOpen.value = !isSidebarOpen.value;
};

// Function to close sidebar
const closeSidebar = () => {
    isSidebarOpen.value = false;
};
</script>

<template>
    <div class="flex h-screen">


        <!-- Main Content -->
        <div class="flex-1 flex flex-col">


            <!-- Page Content -->
            <main class="flex-1 overflow-y-auto">
                <div class="min-h-screen bg-gray-50 p-6">
                    <div class="max-w-5xl mx-auto">
                        <slot />
                    </div>
                </div>
            </main>
        </div>
    </div>
</template>