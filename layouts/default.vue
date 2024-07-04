<template>
  <div
    :class="{ dark: isDarkMode }"
    class="min-h-screen transition-colors duration-500 bg-gray-100 dark:bg-gray-900"
  >
    <!-- WinUI Taskbar -->
    <div
      class="taskbar bg-gray-200 dark:bg-gray-800 flex items-center justify-between px-4 py-2"
    >
      <span class="text-xl font-semibold text-gray-900 dark:text-white"
        >Western Reader</span
      >
      <button
        @click="toggleDarkMode"
        class="toggle-dark-mode bg-gray-300 dark:bg-gray-700 px-2 py-1 rounded text-gray-900 dark:text-white"
      >
        Toggle Dark Mode
      </button>
    </div>
    <slot />
  </div>
</template>

<script lang="ts">
import { defineComponent, onMounted, ref, computed } from "vue";

export default defineComponent({
  setup() {
    // Initialize isDarkMode as a ref with a default value
    const isDarkMode = ref(true);

    // Define toggleDarkMode function
    const toggleDarkMode = () => {
      if (typeof window !== "undefined") {
        document.documentElement.classList.toggle("dark");
        // Update isDarkMode based on the class presence
        isDarkMode.value = document.documentElement.classList.contains("dark");
      }
    };

    // Use onMounted to ensure this code runs client-side only
    onMounted(() => {
      if (typeof window !== "undefined") {
        // Set initial state based on the class presence
        toggleDarkMode();
      }
    });

    return { toggleDarkMode, isDarkMode };
  },
});
</script>
