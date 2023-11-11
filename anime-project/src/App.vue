<template>
  <br>
  <div class="flex items-center justify-center ">
    <div class="flex border-2 border-gray-200 rounded">
        <input type="text" class="px-4 py-2 w-80" placeholder="Search...">
        <button class="px-4 text-white bg-blue-400 border-l ">
            Search
        </button>
    </div>
</div>
<br>
  <div class="w-full">
        <div class="grid grid-cols-1 md:grid-cols-3 gap-4">
            <div v-for="anime in animes" :key="anime.mal_id" class="w-72 border h-80 rounded-lg">
                <div class="relative">
                    <img
                    :src="anime.images.jpg.large_image_url"
                    class="w-full h-80 rounded-lg"
                    alt=""
                    srcset=""
                    />
                    <div class="absolute bottom-0 bg-[rgba(0,0,0,0.7)] w-full flex justify-center items-center rounded-lg p-4">
                        <p class="text-white text-sm">{{ anime.title }}</p>
                    </div>
                </div>
            </div>
        </div>
        <PaginationGlobal
            :pages="pagination.pages"
            :currentPage="pagination.currentPage"
            @method="on_change_page"
            :next="pagination.next_page"
            :prev="pagination.prev_page"
            :total-pag="pagination.total"
        />
    </div>
</template>

<script lang="ts" setup>
  import axios from "axios";
import { onMounted, ref } from "vue";
import { type Data, type GetAnimes } from "./types/images.types";
import { paginate } from './utils';
import PaginationGlobal from './components/search.vue';

interface Pagination {
  pages: Array<string | number>,
  currentPage: number,
  total: number,
  next_page: number,
  prev_page: number
}

const animes = ref<Data[]>([]);

const pagination = ref<Pagination>({
  pages: [],
  currentPage: 1,
  next_page: 2,
  prev_page: 0,
  total: 0
});

const get_animes = async (page = 1, limit = 6) => {
  axios.get<GetAnimes>(`https://api.jikan.moe/v4/anime?page=${page}&limit=${limit}`).then(({data}) => {
    animes.value = data.data;
    pagination.value = {
      pages: paginate(data.pagination.current_page,data.pagination.last_visible_page,1),
      currentPage: data.pagination.current_page,
      next_page: page === data.pagination.last_visible_page ? 0 : page + 1,
      prev_page: page === 1 ? 0 : page - 1,
      total: data.pagination.last_visible_page
    }
  });
};

const on_change_page = (page: number) => {
  get_animes(page);
}

onMounted(async () => {
 await get_animes();
})
</script>
