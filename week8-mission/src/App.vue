<script setup>
import { ref, computed } from 'vue'
import MovieCard from './components/MovieCard.vue'
import AddMovieForm from './components/AddMovieForm.vue'

const movies = ref([
  {
    id: 1,
    title: '인셉션',
    rating: 9.5,
    likes: 0,
    poster: 'https://picsum.photos/seed/inception/300/450'
  },
  {
    id: 2,
    title: '어바웃 타임',
    rating: 9.2,
    likes: 0,
    poster: 'https://picsum.photos/seed/abouttime/300/450'
  },
  {
    id: 3,
    title: '다크 나이트',
    rating: 9.2,
    likes: 0,
    poster: 'https://picsum.photos/seed/darknight/300/450'
  },
  {
    id: 4,
    title: '기생충',
    rating: 8.9,
    likes: 0,
    poster: 'https://picsum.photos/seed/parasite/300/450'
  }
])

const sortType = ref('none')

const sortedMovies = computed(() => {
  if (sortType.value === 'rating') {
    return [...movies.value].sort((a, b) => b.rating - a.rating)
  }
  if (sortType.value === 'likes') {
    return [...movies.value].sort((a, b) => b.likes - a.likes)
  }
  return movies.value
})

const handleLike = (targetId) => {
  const movie = movies.value.find(m => m.id === targetId)
  if (movie) movie.likes++
}

const handleDelete = (targetId) => {
  movies.value = movies.value.filter(m => m.id !== targetId)
}

const handleEdit = (updated) => {
  const movie = movies.value.find(m => m.id === updated.id)
  if (!movie) return
  movie.title = updated.title
  movie.rating = updated.rating
}

const handleAddMovie = (newMovie) => {
  const maxId = movies.value.length > 0
    ? Math.max(...movies.value.map(m => m.id))
    : 0
  movies.value.push({
    id: maxId + 1,
    title: newMovie.title,
    rating: newMovie.rating,
    likes: 0,
    poster: newMovie.poster || `https://picsum.photos/seed/movie${maxId + 1}/300/450`
  })
}
</script>

<template>
  <div class="container">
    <header class="page-header">
      <h1 class="page-title">MOVIE</h1>
      <p class="page-sub">Props &amp; Emit 실습</p>
    </header>

    <!-- 정렬 버튼 -->
    <div class="sort-area">
      <span class="sort-label">정렬</span>
      <button
        class="sort-btn"
        :class="{ active: sortType === 'rating' }"
        @click="sortType = 'rating'"
      >⭐ 평점순</button>
      <button
        class="sort-btn"
        :class="{ active: sortType === 'likes' }"
        @click="sortType = 'likes'"
      >❤️ 좋아요순</button>
      <button
        v-if="sortType !== 'none'"
        class="sort-btn reset-btn"
        @click="sortType = 'none'"
      >✖ 초기화</button>
    </div>

    <AddMovieForm @add-movie="handleAddMovie" />

    <main class="movie-grid">
      <MovieCard
        v-for="m in sortedMovies"
        :key="m.id"
        :movie="m"
        @like-movie="handleLike"
        @delete-movie="handleDelete"
        @edit-movie="handleEdit"
      />
    </main>

    <!-- 영화가 하나도 없을 때 -->
    <div v-if="sortedMovies.length === 0" class="empty-state">
      <p class="empty-icon">🎬</p>
      <p class="empty-msg">등록된 영화가 없습니다.</p>
      <p class="empty-sub">위 폼에서 새 영화를 추가해보세요.</p>
    </div>
  </div>
</template>

<style>
body {
  background-color: #141414;
  margin: 0;
}
</style>

<style scoped>
.container {
  padding: 48px 40px 60px;
  font-family: 'Helvetica Neue', Arial, sans-serif;
  max-width: 1080px;
  margin: 0 auto;
  min-height: 100vh;
}

.page-header {
  text-align: center;
  margin-bottom: 36px;
}
.page-title {
  font-size: 2.4rem;
  font-weight: 900;
  color: #e50914;
  letter-spacing: 4px;
  margin: 0 0 6px 0;
}
.page-sub {
  font-size: 0.8rem;
  color: #555;
  letter-spacing: 1px;
  margin: 0;
  text-transform: uppercase;
}

/* 정렬 */
.sort-area {
  display: flex;
  align-items: center;
  gap: 8px;
  margin-bottom: 20px;
  flex-wrap: wrap;
}
.sort-label {
  font-size: 0.78rem;
  color: #666;
  font-weight: 600;
  letter-spacing: 1px;
  text-transform: uppercase;
  margin-right: 4px;
}
.sort-btn {
  padding: 7px 16px;
  border: 1px solid #333;
  border-radius: 20px;
  background: transparent;
  color: #aaa;
  font-size: 0.82rem;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.18s ease;
}
.sort-btn:hover {
  border-color: #888;
  color: #e5e5e5;
}
.sort-btn.active {
  background-color: #e50914;
  border-color: #e50914;
  color: #fff;
}
.reset-btn {
  color: #555;
  border-color: #2a2a2a;
  font-size: 0.78rem;
}
.reset-btn:hover {
  border-color: #e50914;
  color: #e50914;
}

.movie-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(230px, 1fr));
  gap: 20px;
}

.empty-state {
  text-align: center;
  padding: 60px 0;
}
.empty-icon {
  font-size: 2.5rem;
  margin: 0 0 12px 0;
}
.empty-msg {
  font-size: 1rem;
  color: #555;
  margin: 0 0 6px 0;
}
.empty-sub {
  font-size: 0.82rem;
  color: #3a3a3a;
  margin: 0;
}
</style>
