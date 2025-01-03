<script setup>
import { ref, onMounted } from 'vue'
import axios from 'axios'

const movies = ref([])
const fetchMovies = async () => {
  try {
    const response = await axios.get('https://api.themoviedb.org/3/movie/popular', {
      params: {
        api_key: 'f57b89bf23976c945a676bd0553ba17b',
        page: 1
      }
    })
    movies.value = response.data
  } catch (error) {
    console.error('Error fetching movies:', error)
  }
}

onMounted(() => {
  fetchMovies()
})

</script>

<template>
  <div class="p-4">
    <div class="grid grid-cols-3 items-center">
      <div class="text-gray-900 text-3xl font-bold m-4 col-span-1">Movies</div>
      <div class="col-span-2">
        <input type="text" class="w-full p-2 border border-gray-200 rounded-xl" placeholder="Search movies...">
      </div>
    </div>
    <div class="grid lg:grid-cols-7 md:grid-cols-2 grid-cols-1 gap-4">
      <div v-for="movie in movies.results" class="rounded-xl p-4 border border-gray-200 shadow-xl">
        <img :src="'https://image.tmdb.org/t/p/original/' + movie.poster_path" alt="" class="mb-3">
        <div class="text-xs font-semibold">{{ movie.original_title }}</div>
        <div class="text-xs">Rating : <span class="font-semibold">{{ (Math.round(movie.vote_average * 10)) }}</span>
          ({{
            movie.vote_count }} vote)</div>
      </div>
    </div>
  </div>
</template>
