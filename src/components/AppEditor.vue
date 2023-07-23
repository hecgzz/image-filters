<template>
  <div class="my-8">
    <div class="flex flex-row">
      <div class="basis-3/4 pr-4">
        <canvas class="w-full" ref="canvasEl"></canvas>
        <a class="bg-indigo-700 py-4 block w-full mt-2 text-center text-white text-xl" :href="canvasImgURL"
          download="image.png">
          Download
        </a>
        <h2 class="pt-8 text-2xl text-indigo-700">Instuctions</h2>
        <div class="pt-4">
          <p class="text-l">1. Select a filter a filter from the sidebar</p>
          <p class="text-l">&nbsp; &nbsp; To revert the filter use the button again</p>
          <p class="text-l pt-4">2. Click the Download button to get the filtered image</p>
        </div>

      </div>
      <div class="basis-1/4">
        <h2 class="text-center text-2xl text-indigo-700">Filters</h2>
        <div class="text-white text-xl mt-4">
          <div class="grid grid-cols-2 justify-center gap-4">
            <button type="button" class="py-4 w-full" v-for="(filter, index) in filters" :key="index" :class="{
              'bg-pink-600': store.filter !== filter,
              'bg-green-600': store.filter === filter,
            }" @click="store.filter = store.filter === filter ? '' : filter">{{ filter }}
            </button>
          </div>
        </div>
        <div>
          <font-awesome-icon :icon="['fas', 'droplet']" />
          <h2 class="pt-8 text-center text-2xl text-indigo-700">Tints</h2>
        </div>
        <div class="text-white text-xl mt-4">
          <div class="grid grid-cols-2 justify-center gap-4">
            <button type="button" class="py-4 px-2" v-for="(filter, index) in tints" :key="index" :class="{
              'bg-pink-600': store.filter !== filter,
              'bg-green-600': store.filter === filter,
            }" @click="store.filter = store.filter === filter ? '' : filter">{{ filter }}
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { useImageStore } from '@/stores/image';
import useReader from "@/composables/use-reader";
import useCanvas from '@/composables/use-canvas';

const filters = ["oceanic", "seagren", "flagblue", "diamante", "vintage", "perfume", "serenity", "lofi", "cali", "obsidian", "firenze", "dramatic", "golden", "pastelpink"];
const tints = ["bluechrome", "rosetint", "mauve", "radio"];
const store = useImageStore();
const { canvasEl, loadImage, drawOriginalImage, filterImage, canvasImgURL } = useCanvas();
const { reader } = useReader(store.file, () => {
  if (!reader.result) return;

  const dataURL = reader.result.toString();
  loadImage(dataURL);
});

store.$subscribe((mutation, state) => {
  drawOriginalImage();
  console.log("reset")
  filterImage(state.filter);
})
</script>