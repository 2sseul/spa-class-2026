<script setup>
import { ref } from 'vue'

const props = defineProps({
  movie: {
    type: Object,
    required: true
  }
})

const emit = defineEmits(['like-movie', 'delete-movie', 'edit-movie'])

const isEditing = ref(false)
const editTitle = ref('')
const editRating = ref(0)
const errorMsg = ref('')

function startEdit() {
  editTitle.value = props.movie.title
  editRating.value = props.movie.rating
  errorMsg.value = ''
  isEditing.value = true
}

function saveEdit() {
  if (!editTitle.value.trim()) {
    errorMsg.value = '제목을 입력해주세요.'
    return
  }
  if (editRating.value === '' || editRating.value === null) {
    errorMsg.value = '평점을 입력해주세요.'
    return
  }
  const ratingNum = Number(editRating.value)
  if (isNaN(ratingNum)) {
    errorMsg.value = '평점은 숫자로 입력해주세요.'
    return
  }
  if (ratingNum < 0 || ratingNum > 10) {
    errorMsg.value = '평점은 0에서 10 사이로 입력해주세요.'
    return
  }

  emit('edit-movie', {
    id: props.movie.id,
    title: editTitle.value.trim(),
    rating: parseFloat(ratingNum.toFixed(1))
  })

  isEditing.value = false
  errorMsg.value = ''
}

function cancelEdit() {
  isEditing.value = false
  errorMsg.value = ''
}
</script>

<template>
  <div class="card" :class="{ 'is-editing': isEditing }">

    <div class="poster-area">
      <img :src="movie.poster" :alt="movie.title" class="poster" />
    </div>

    <div class="content-area">

      <template v-if="!isEditing">
        <h3 class="title">{{ movie.title }}</h3>
        <div class="meta">
          <span class="rating">⭐ {{ Number(movie.rating).toFixed(1) }}</span>
          <span class="divider">·</span>
          <span class="likes">❤️ {{ movie.likes }}</span>
        </div>
        <div class="btn-group">
          <button class="btn like-btn" @click="$emit('like-movie', movie.id)">추천</button>
          <button class="btn edit-btn" @click="startEdit">수정</button>
          <button class="btn del-btn" @click="$emit('delete-movie', movie.id)">삭제</button>
        </div>
      </template>

      <template v-else>
        <div class="edit-form">
          <input
            v-model="editTitle"
            type="text"
            class="edit-input"
            placeholder="영화 제목"
          />
          <input
            v-model="editRating"
            type="number"
            class="edit-input"
            min="0"
            max="10"
            step="0.1"
            placeholder="평점 0 ~ 10"
          />
          <p v-if="errorMsg" class="error-msg">{{ errorMsg }}</p>
          <div class="edit-actions">
            <button class="cancel-btn" @click="cancelEdit">취소</button>
            <button class="save-btn" @click="saveEdit">저장</button>
          </div>
        </div>
      </template>

    </div>
  </div>
</template>

<style scoped>
.card {
  background: #181818;
  border-radius: 6px;
  overflow: hidden;
  border: 1px solid #242424;
  transition: transform 0.2s ease, box-shadow 0.2s ease;
}
.card:hover {
  transform: scale(1.03);
  box-shadow: 0 10px 32px rgba(0, 0, 0, 0.6);
  border-color: #333;
}
.card.is-editing {
  transform: none !important;
  border: 1px solid #e50914;
  box-shadow: 0 0 0 1px #e50914;
}

.poster-area {
  width: 100%;
  height: 290px;
}
.poster {
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;
}

.content-area {
  padding: 14px 16px 18px;
}

.title {
  margin: 0 0 8px 0;
  font-size: 0.95rem;
  font-weight: 700;
  color: #e5e5e5;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.meta {
  display: flex;
  align-items: center;
  gap: 6px;
  margin-bottom: 14px;
  font-size: 0.82rem;
}
.rating {
  color: #f5c518;
  font-weight: 600;
}
.divider {
  color: #444;
}
.likes {
  color: #e50914;
  font-weight: 600;
}

.btn-group {
  display: flex;
  gap: 7px;
}
.btn {
  flex: 1;
  padding: 7px 0;
  cursor: pointer;
  border-radius: 4px;
  font-size: 0.78rem;
  font-weight: 700;
  transition: opacity 0.15s, background 0.15s;
  border: none;
  letter-spacing: 0.3px;
}
.btn:hover { opacity: 0.85; }
.like-btn { background: #1b5b91; color: #e5e5e5; }
.edit-btn { background: #7a5c00; color: #f5c518; }
.del-btn  { background: #6b0000; color: #ff6b6b; }

.edit-form {
  display: flex;
  flex-direction: column;
  gap: 8px;
}
.edit-input {
  padding: 9px 12px;
  background: #111;
  border: 1px solid #2e2e2e;
  border-radius: 4px;
  font-size: 0.88rem;
  color: #e5e5e5;
  width: 100%;
  box-sizing: border-box;
  outline: none;
  transition: border-color 0.15s;
}
.edit-input:focus {
  border-color: #e50914;
}
.edit-input::placeholder {
  color: #444;
}
.error-msg {
  color: #ff6b6b;
  font-size: 0.76rem;
  margin: 0;
}
.edit-actions {
  display: flex;
  gap: 7px;
  margin-top: 2px;
}
.save-btn {
  flex: 2;
  padding: 8px 0;
  background: #e50914;
  color: #fff;
  border: none;
  border-radius: 4px;
  font-size: 0.82rem;
  font-weight: 700;
  cursor: pointer;
  letter-spacing: 0.3px;
  transition: background 0.15s;
}
.save-btn:hover {
  background: #f40612;
}
.cancel-btn {
  flex: 1;
  padding: 8px 0;
  background: transparent;
  color: #666;
  border: 1px solid #2e2e2e;
  border-radius: 4px;
  font-size: 0.82rem;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.15s;
}
.cancel-btn:hover {
  border-color: #555;
  color: #aaa;
}
</style>
