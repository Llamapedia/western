<template>
  <div class="p-5 rounded-lg shadow-md">
    <div class="flex justify-between items-center mb-4">
      <div class="flex items-center gap-4">
        <button
          @click="goBack"
          class="bg-gray-300 hover:bg-gray-400 text-gray-800 font-bold py-2 px-4 rounded-l mr-2"
        >
          Go Back
        </button>
        <h1 class="text-xl font-semibold text-gray-900 dark:text-gray-100">
          {{ comicTitle }}
        </h1>
      </div>
      <button
        class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded"
        @click="openReader"
      >
        Read
      </button>
    </div>
    <div class="grid grid-cols-6 gap-4" v-if="comicPages">
      <div class="comic-page" v-for="page in comicPages" :key="page">
        <img
          :src="`/comic/${comicId}/${page}.jpg`"
          alt="Comic Page"
          class="w-full h-auto rounded-lg shadow"
        />
      </div>
    </div>
  </div>
  <ComicReader
    :comicId="comicId"
    :initialPage="1"
    :totalPages="comicPages"
    :visible="readerVisible"
    @update:visible="readerVisible = $event"
  />
</template>

<script setup lang="ts">
import { useRoute, useRouter } from "vue-router";

const route = useRoute();
const router = useRouter();

const comicId = ref<number | null>();
const comics = ref<Comic[]>([]);
const comicTitle = ref<String>("");
const comicPages = ref<number>(0);

const readerVisible = ref<boolean>(false);

interface Comic {
  id: number;
  title: string;
  pages: number;
}

const goBack = () => {
  router.back();
};

onMounted(async () => {
  comicId.value = route.params.id ? parseInt(route.params.id as string) : null;

  const response = await fetch("/comics.json");
  const data = await response.json();
  comics.value = data;

  const currentComic = comics.value.find((comic) => comic.id === comicId.value);
  if (currentComic) {
    comicTitle.value = currentComic.title;
    comicPages.value = currentComic.pages;
  }
});

function openReader() {
  readerVisible.value = true;
}
</script>

<style scoped>
.winui-container {
  padding: 20px;
  background-color: #f0f0f0;
  border-radius: 8px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

.title-bar {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
}

.comic-title {
  margin: 0;
  font-size: 24px;
  color: #202020;
}

.read-button {
  padding: 10px 20px;
  font-size: 16px;
  color: #ffffff;
  background-color: #0078d4;
}
</style>
