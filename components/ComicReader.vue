<template>
  <div
    ref="readerContainer"
    v-show="visible"
    class="inset-0 flex justify-center items-center select-none"
    @click="handleClick"
  >
    <img :src="currentImageUrl" class="max-w-full max-h-full h-full" />
  </div>
</template>

<script setup>
import {
  ref,
  watch,
  onMounted,
  onUnmounted,
  defineProps,
  defineEmits,
  watchEffect,
} from "vue";

const props = defineProps({
  comicId: Number,
  initialPage: Number,
  totalPages: Number,
  visible: Boolean,
});

const emit = defineEmits(["update:visible"]);
const readerContainer = ref(null);

const currentImageUrl = ref("");
const currentPage = ref(props.initialPage);

watchEffect(() => {
  currentImageUrl.value = `/comic/${props.comicId}/${currentPage.value}.jpg`;
  preloadImages();
});

watch(
  () => props.visible,
  (newVal) => {
    if (newVal) enterFullScreen();
    else exitFullScreen();
  }
);

function handleClick(event) {
  const { clientX } = event;
  const { innerWidth } = window;
  if (clientX < innerWidth / 2) {
    if (currentPage.value > 1) currentPage.value--;
    else currentPage.value = props.totalPages; // Wrap to last page
  } else {
    if (currentPage.value < props.totalPages) currentPage.value++;
    else currentPage.value = 1; // Wrap to first page
  }
}

function preloadImages() {
  for (let i = 1; i <= 3; i++) {
    const nextPage =
      (currentPage.value + i) % props.totalPages || props.totalPages;
    const img = new Image();
    img.src = `/comic/${props.comicId}/${nextPage}.jpg`;
  }
}

async function enterFullScreen() {
  if (readerContainer.value && readerContainer.value.requestFullscreen) {
    await readerContainer.value.requestFullscreen();
  }
}

async function exitFullScreen() {
  if (document.fullscreenElement && document.exitFullscreen) {
    await document.exitFullscreen();
    props.visible = false;
  }
}

onUnmounted(() => {
  exitFullScreen();
});
</script>
