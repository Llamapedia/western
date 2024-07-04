<template>
  <div class="grid grid-cols-5 gap-4 mt-4">
    <div
      v-for="comic in comics"
      :key="comic.id"
      class="comic-container p-4 rounded-lg shadow bg-white dark:bg-gray-800"
      @click="() => $router.push(`/${comic.id}`)"
    >
      <!-- Display the first image of each comic -->
      <img
        :src="`/comic/${comic.id}/1.jpg`"
        alt="Comic Image"
        class="comic-image mb-2"
      />
      <h3 class="font-bold dark:text-white">{{ comic.title }}</h3>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, onMounted, ref, computed } from "vue";

interface Comic {
  id: number;
  title: string;
}

export default defineComponent({
  setup() {
    const comics = ref<Comic[]>([]);
    const isDarkMode = computed(() =>
      document.documentElement.classList.contains("dark")
    );

    onMounted(async () => {
      const response = await fetch("/comics.json");
      comics.value = await response.json();
    });

    const toggleDarkMode = () => {
      document.documentElement.classList.toggle("dark");
    };

    return { comics, toggleDarkMode, isDarkMode };
  },
});
</script>

<style scoped>
/* Responsive Grid and Image Styling */
.comic-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  overflow: hidden;
  max-width: 250px;
  margin: 0 auto;
  padding: 16px;
  border-radius: 8px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  text-align: left;
}

.comic-image {
  max-width: 100%;
  height: auto;
  border-radius: 4px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  display: block;
  max-width: 100%;
  max-height: 400px;
  margin: 0 auto;
  object-fit: cover;
}

/* Adjust grid template columns for responsiveness */
@media (min-width: 640px) {
  /* sm */
  .grid {
    grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
  }
}

@media (min-width: 768px) {
  /* md */
  .grid {
    grid-template-columns: repeat(auto-fit, minmax(180px, 1fr));
  }
}

@media (min-width: 1024px) {
  /* lg */
  .grid {
    grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
  }
}

@media (min-width: 1280px) {
  /* xl */
  .grid {
    grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  }
}
</style>
