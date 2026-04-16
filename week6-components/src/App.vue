<script setup>
import { ref } from 'vue';
import PageHeader from './components/PageHeader.vue';
import MovieCard from './components/MovieCard.vue'; 

const movies = ref([
{  id: 1, title: '인셉션', rating: 9.5,
  poster: "https://i.namu.wiki/i/ujkt6TPVoeYxq3q9h2yE_MckhgdJ6rOcdgnkCcs1ivWzB5hlnPBCEzatFDMApkNfWlxuCTQYH3d2SMj632Fzsg.webp",
  isHot: true, isNew: false, tags: ['SF', '명작']
},
{  id: 2, title: '어바웃 타임', rating: 9.2,
  poster: "https://i.namu.wiki/i/LzoxaAtXeLBOtibjJw34YNxQB8peseMhLmC9GLzAO01_1GOZMHDh9jDR_HbuWwbDS6jwdkkH3eV1mmeYnGvhbQ.webp",
  isHot: false, isNew: false, tags: ['로맨스', '인생영화']
},
{  id: 3, title: '다크 나이트', rating: 9.8,
  poster: "https://i.namu.wiki/i/tvjifOxt40saLEdUAOD1f150KXOOOti08v2KwiF4Z3NjPq01674QKLUeTNP2VOB-xQKu8hD7svp3vWoCAoa1Sg.jpg",
  isHot: true, isNew: true, tags: ['조커', '히어로']
},
{  id: 4, title: '라라랜드', rating: 9.0,
  poster: "https://i.namu.wiki/i/78uTXq-Jd3ME_MYXtiyOo-qBPjwpiNF9qs1ko9YvE1BmaVagE9-h95a5Xuh0jVt6WX9sY8seQLZlU2GidF7Gcg.webp",
  isHot: false, isNew: true, tags: ['뮤지컬', '로맨스']
}]);
</script>
<template>
  <div class="container">
    <PageHeader title="🍿 6주차 영화 추천 시스템" :count="movies.length"/>
    <main class="movie-grid">
      <MovieCard v-for="m in movies" :key="m.id" :movie="m" />
    </main>
  </div>
</template>

<style scoped>
  .container { 
    padding: 50px; 
    max-width: 1100px; 
    margin: 0 auto; 
    background-color: #f1f2f6; 
    min-height: 100vh; 
  }
  .movie-grid { 
    display: grid; 
    grid-template-columns: repeat(auto-fill, minmax(250px, 1fr)); 
    gap: 35px; 
  }
</style>