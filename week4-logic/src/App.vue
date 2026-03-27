<script setup>
import {ref} from 'vue';

const searchInput = ref('');
const searchKeyword = ref('');
const currentTab = ref('전체');

const isModalOpen = ref(false);
const selectedMovie = ref(null);

const movies = ref([
  {id: 1, title: '다크 나이트', genre: '액션', rating:9.5, poster: 'https://placehold.co/150x220/222222/FFFFFF?text=Dark+Knight%27'},
  {id: 2, title: '인터스텔라', genre: 'SF', rating:8.6, poster: 'https://placehold.co/150x220/000080/FFFFFF?text=Interstellar'},
  {id: 3, title: '어바웃 타임', genre: '로맨스', rating:7.5, poster: 'https://placehold.co/150x220/FFB6C1/FB0000?text=About+Time'},
  {id: 4, title: '인셉션', genre: 'SF', rating:9.2, poster: 'https://placehold.co/150x220/808080/FFFFFF?text=Inception'},
  {id: 5, title: '라라랜드', genre: '로맨스', rating:6.8, poster: 'https://placehold.co/150x220/FFFF00/000000?text=La+La_Land'},
])

const openModal = (movie) => {
  selectedMovie.value = movie;
  isModalOpen.value = true;
}

const deleteMovie = (targetId) => {
  movies.value = movies.value.filter(movie => movie.id !== targetId);

  isModalOpen.value = false;
}

</script>

<template>
  <div class="container">
    <h1>영화 데이터베이스</h1>
    <div class="search-area">
      <input type="text" v-model="searchInput" @keyup.enter="searchKeyword = searchInput" placeholder="영화 제목 검색 후 엔터"/>
      <button @click="searchKeyword = searchInput">검색</button>
    </div>
    <div class="tabs">
      <button @click="currentTab = '전체'" :class="{active: currentTab === '전체'}">전체</button> 
      <button @click="currentTab = '액션'" :class="{active: currentTab === '액션'}">액션</button> 
      <button @click="currentTab = 'SF'" :class="{active: currentTab === 'SF'}">SF</button> 
      <button @click="currentTab = '로맨스'" :class="{active: currentTab === '로맨스'}">로맨스</button> 
    </div>
    <div class="movie-grid">
      <div v-for="movie in movies" :key="movie.id" class="card" v-show="movie.title.includes(searchKeyword)">
        <img :src="movie.poster" alt="포스터">
        <h3>{{ movie.title }}</h3>
        <p>⭐️ {{ movie.rating }} / {{ movie.genre }}</p>

        <div class="badge-area">
          <span v-if="movie.rating >= 9">👑 명작</span>
          <span v-else-if="movie.rating >= 7">👍🏻 추천</span>
          <span v-else>🤔 킬링타임</span>
        </div>

        <button @click="openModal(movie)">상세보기</button>
      </div>

    </div>
    <div v-if="isModalOpen" class="modal-bg" @click="isModalOpen = false">
      <div class="modal-content" @click.stop>
        <h2>{{ selectedMovie.title }} 상세정보</h2>
        <img :src="selectedMovie.poster" alt="포스터" />
        <p>장르: {{ selectedMovie.genre }}</p>
        <p>평점: {{ selectedMovie.rating }}</p>
        <button @click="isModalOpen = false">닫기</button>
        <button class="delete-btn" @click="deleteMovie(selectedMovie.id)">
          🗑️ 삭제하기
        </button>
      </div>
    </div>
  </div>
</template>

<style scoped>
.container {
  max-width: 900px;
  margin: 0 auto;
  padding: 20px;
  text-align: center;
  font-family: sans-serif;
}

.search-area input {
  padding: 10px;
  width: 260px;
  border: 1px solid #ccc;
  border-radius: 6px;
  margin-right: 8px;
  font-size: 15px;
}

.search-area button {
  padding: 10px 20px;
  background-color: #333;
  color: white;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  font-weight: bold;
}

.tabs button {
  padding: 10px 20px;
  border: 1px solid #ddd;
  background: #f9f9f9;
  cursor: pointer;
  border-radius: 8px;
  transition: 0.2s;
  font-size: 15px;
}

.tabs button.active {
  background: #4288e3;
  color: white;
  border-color: #4288e3;
  font-weight: bold;
}

.movie-grid {
  display: flex;
  flex-wrap: wrap;
  gap: 30px;
  justify-content: center;
}

.card {
  border: 1px solid #000;
  padding: 20px;
  border-radius: 12px;
  width: 220px;
  text-align: center;
  box-shadow: 0 4px 6px rgba(0,0,0,0.05);
  transition: 0.3s;
  background: white;
}

.card img {
  width: 100%;
  border-radius: 8px;
  margin-bottom: 10px;
}

.card h3 {
  margin: 10px 0 5px 0;
}

.badge-area {
  margin: 15px 0;
  font-weight: bold;
  font-size: 14px;
}

.badge {
  padding: 5px 10px;
  border-radius: 20px;
}

.badge.master {
  background-color: #f70700;
  color: #000a0b;
}

.badge.good {
  background-color: #ae0f7a;
  color: #00030f;
}

.badge.soso {
  background-color: #eeeeee;
  color: #816161;
}

.detail-btn {
  width: 100%;
  padding: 10px;
  background: #4bb633;
  color: white;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  font-weight: bold;
  margin-top: 5px;
}

.modal-bg {
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  background: rgba(0,0,0,0.6);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1000;
}

.modal-content {
  background: white;
  padding: 30px;
  border-radius: 16px;
  width: 320px;
  text-align: center;
  box-shadow: 0 10px 20px rgba(0,0,0,0.2);
}

.modal-content img {
  width: 150px;
  border-radius: 8px;
  margin-bottom: 15px;
}

.modal-actions {
  display: flex;
  justify-content: center;
  gap: 10px;
  margin-top: 25px;
}

.modal-actions button {
  padding: 10px 20px;
  border: none;
  border-radius: 0px;
  cursor: pointer;
  font-weight: bold;
}

.close-btn {
  background: #eeeeee;
  color: #537;
}

.delete-btn {
  background: #633333;
  color: white;
}
</style>