<template>
  <div></div>
  <div>
    <ul class="flex md:hidden mt-5 justify-between">
      <li>
        <button @click="changePage(prev)"
          class="mx-1 bg-blue-500 cursor-pointer rounded-full p-2 w-12 h-12 text-center text-lg text-white"
          :disabled="currentPage === 1">
          <i class="fas fa-chevron-left"></i>
        </button>
      </li>
      <li class="flex justify-center items-center">
        <span>Pagina {{ currentPage }} de {{ totalPag }}</span>
      </li>
      <li>
        <button class="mx-1 bg-blue-500 rounded-full p-2 w-12 h-12 cursor-pointer text-center text-lg text-white"
          :disabled="currentPage === totalPag" @click="changePage(next)">
          <i class="fas fa-chevron-right"></i>
        </button>
      </li>
    </ul>
    <ul class="hidden md:flex mt-6">
      <li>
        <button @click="changePage(prev)"
          class="mx-1 bg-blue-500 cursor-pointer rounded-full p-2 w-12 h-12 text-center text-lg text-white"
          :disabled="currentPage === 1">
          <svg xmlns="http://www.w3.org/2000/svg" height="2em" viewBox="0 0 320 512">
            <!--! Font Awesome Free 6.4.2 by @fontawesome - https://fontawesome.com License - https://fontawesome.com/license (Commercial License) Copyright 2023 Fonticons, Inc. -->
            <path
              d="M9.4 233.4c-12.5 12.5-12.5 32.8 0 45.3l192 192c12.5 12.5 32.8 12.5 45.3 0s12.5-32.8 0-45.3L77.3 256 246.6 86.6c12.5-12.5 12.5-32.8 0-45.3s-32.8-12.5-45.3 0l-192 192z" />
          </svg>
        </button>
      </li>
      <li
        class="mx-1 border font-semibold border-blue rounded-full p-2 w-12 h-12 text-center text-[10px] flex justify-center items-center cursor-pointer"
        v-for="page in pages" :class="page === currentPage ? 'bg-blue-500 text-white' : ' text-blue-500'
          " v-bind:key="page" @click="changePage(page)">
        {{ page }}
      </li>
      <li>
        <button class="mx-1 bg-blue-500 rounded-full p-2 w-12 h-12 cursor-pointer text-center text-lg text-white"
          :disabled="currentPage === totalPag" @click="changePage(next)">
          <svg xmlns="http://www.w3.org/2000/svg" class="position:" height="2em" viewBox="-10 60 320 512">
            <!--! Font Awesome Free 6.4.2 by @fontawesome - https://fontawesome.com License - https://fontawesome.com/license (Commercial License) Copyright 2023 Fonticons, Inc. -->
            <path
              d="M310.6 233.4c12.5 12.5 12.5 32.8 0 45.3l-192 192c-12.5 12.5-32.8 12.5-45.3 0s-12.5-32.8 0-45.3L242.7 256 73.4 86.6c-12.5-12.5-12.5-32.8 0-45.3s32.8-12.5 45.3 0l192 192z" />
          </svg>
        </button>
      </li>
    </ul>
  </div>
</template>

<script lang="ts" setup>
interface Props {
  pages: Array<number | string>;
  currentPage: number;
  totalPag: number;
  next: number;
  prev: number;
}
const { pages, currentPage, totalPag, next, prev } = defineProps<Props>();

type Emits = {
  (event: "method", page: number): void;
};

const emits = defineEmits<Emits>();

const changePage = (page: number | string) => {
  emits("method", Number(page));
};
</script>
