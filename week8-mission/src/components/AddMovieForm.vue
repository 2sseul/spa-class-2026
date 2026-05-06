<script setup>
import { ref, computed } from 'vue'

const emit = defineEmits(['add-movie'])

const newTitle = ref('')
const newRating = ref('')

const imageMode = ref('url')
const imageUrl = ref('')
const previewUrl = ref('')
const imageError = ref(false)

const fileInputRef = ref(null)

const currentPreview = computed(() => {
  if (imageMode.value === 'url') return imageUrl.value.trim()
  return previewUrl.value
})

function switchMode(mode) {
  imageMode.value = mode
  imageError.value = false
  if (mode === 'url') {
    previewUrl.value = ''
    if (fileInputRef.value) fileInputRef.value.value = ''
  } else {
    imageUrl.value = ''
  }
}

function onFileChange(e) {
  const file = e.target.files[0]
  if (!file) {
    previewUrl.value = ''
    return
  }
  if (!file.type.startsWith('image/')) {
    alert('이미지 파일만 선택할 수 있어요.')
    e.target.value = ''
    return
  }
  previewUrl.value = URL.createObjectURL(file)
}

function handleAdd() {
  if (!newTitle.value.trim()) {
    alert('영화 제목을 입력해주세요.')
    return
  }
  if (newRating.value === '') {
    alert('평점을 입력해주세요.')
    return
  }
  const rating = Number(newRating.value)
  if (isNaN(rating)) {
    alert('평점은 숫자로 입력해주세요.')
    return
  }
  if (rating < 0 || rating > 10) {
    alert('평점은 0에서 10 사이로 입력해주세요.')
    return
  }

  let poster = ''
  if (imageMode.value === 'url' && imageUrl.value.trim()) {
    poster = imageUrl.value.trim()
  } else if (imageMode.value === 'file' && previewUrl.value) {
    poster = previewUrl.value
  }

  emit('add-movie', { title: newTitle.value.trim(), rating: parseFloat(rating.toFixed(1)), poster })

  newTitle.value = ''
  newRating.value = ''
  imageUrl.value = ''
  previewUrl.value = ''
  imageError.value = false
  if (fileInputRef.value) fileInputRef.value.value = ''
}
</script>

<template>
  <div class="form-box">
    <p class="form-title">+ 새 영화 추가</p>

    <!-- 이미지 탭 -->
    <div class="mode-toggle">
      <button
        type="button"
        class="toggle-btn"
        :class="{ active: imageMode === 'url' }"
        @click="switchMode('url')"
      >링크</button>
      <button
        type="button"
        class="toggle-btn"
        :class="{ active: imageMode === 'file' }"
        @click="switchMode('file')"
      >파일</button>
    </div>

    <div v-if="imageMode === 'url'" class="image-input-row">
      <input
        v-model="imageUrl"
        type="text"
        class="form-input"
        placeholder="이미지 URL (없으면 기본 이미지 사용)"
        @input="imageError = false"
      />
    </div>

    <div v-else class="image-input-row">
      <label class="file-label">
        <input
          ref="fileInputRef"
          type="file"
          accept="image/*"
          class="file-input-hidden"
          @change="onFileChange"
        />
        <span class="file-btn">파일 선택</span>
        <span class="file-name">{{ previewUrl ? '이미지 선택됨 ✔' : '선택된 파일 없음' }}</span>
      </label>
    </div>

    <div v-if="currentPreview && !imageError" class="preview-box">
      <img
        :src="currentPreview"
        alt="미리보기"
        class="preview-img"
        @error="imageError = true"
      />
      <span class="preview-caption">미리보기</span>
    </div>
    <p v-if="imageError" class="img-error-msg">⚠️ 이미지를 불러올 수 없어요.</p>

    <div class="form-row">
      <input v-model="newTitle" type="text" class="form-input" placeholder="영화 제목" />
      <input
        v-model="newRating"
        type="number"
        class="form-input rating-input"
        min="0"
        max="10"
        step="0.1"
        placeholder="평점 (0~10)"
      />
      <button class="add-btn" @click="handleAdd">등록</button>
    </div>
  </div>
</template>

<style scoped>
.form-box {
  background: #181818;
  border: 1px solid #242424;
  border-radius: 6px;
  padding: 20px 22px;
  margin-bottom: 24px;
}
.form-title {
  margin: 0 0 16px 0;
  font-size: 0.75rem;
  font-weight: 700;
  color: #666;
  letter-spacing: 1.5px;
  text-transform: uppercase;
}

.mode-toggle {
  display: flex;
  gap: 0;
  margin-bottom: 12px;
  border: 1px solid #333;
  border-radius: 4px;
  overflow: hidden;
  width: fit-content;
}
.toggle-btn {
  padding: 6px 18px;
  background: transparent;
  border: none;
  color: #666;
  font-size: 0.8rem;
  font-weight: 600;
  cursor: pointer;
  transition: background 0.15s, color 0.15s;
  letter-spacing: 0.5px;
}
.toggle-btn + .toggle-btn {
  border-left: 1px solid #333;
}
.toggle-btn:hover {
  color: #aaa;
  background: #222;
}
.toggle-btn.active {
  background: #e50914;
  color: #fff;
}

.image-input-row {
  margin-bottom: 10px;
}
.image-input-row .form-input {
  width: 100%;
}

.file-label {
  display: flex;
  align-items: center;
  gap: 10px;
  cursor: pointer;
}
.file-input-hidden {
  display: none;
}
.file-btn {
  padding: 6px 14px;
  background: #242424;
  border: 1px solid #333;
  border-radius: 4px;
  font-size: 0.82rem;
  color: #aaa;
  white-space: nowrap;
  transition: background 0.15s;
}
.file-label:hover .file-btn {
  background: #2e2e2e;
  color: #e5e5e5;
}
.file-name {
  font-size: 0.78rem;
  color: #555;
}

.preview-box {
  display: flex;
  align-items: flex-end;
  gap: 8px;
  margin-bottom: 12px;
}
.preview-img {
  width: 72px;
  height: 100px;
  object-fit: cover;
  border-radius: 4px;
  border: 1px solid #333;
}
.preview-caption {
  font-size: 0.72rem;
  color: #555;
}
.img-error-msg {
  font-size: 0.78rem;
  color: #ff6b6b;
  margin: 0 0 10px 0;
}

.form-row {
  display: flex;
  gap: 10px;
  align-items: center;
  flex-wrap: wrap;
}
.form-input {
  padding: 9px 12px;
  background: #111;
  border: 1px solid #333;
  border-radius: 4px;
  font-size: 0.88rem;
  color: #e5e5e5;
  outline: none;
  flex: 1;
  min-width: 120px;
  box-sizing: border-box;
  transition: border-color 0.15s;
}
.form-input:focus {
  border-color: #e50914;
}
.form-input::placeholder {
  color: #444;
}
.rating-input {
  flex: 0 0 140px;
}
.add-btn {
  padding: 9px 22px;
  background-color: #e50914;
  color: white;
  border: none;
  border-radius: 4px;
  font-weight: 700;
  font-size: 0.88rem;
  cursor: pointer;
  white-space: nowrap;
  letter-spacing: 0.5px;
  transition: background 0.15s;
}
.add-btn:hover {
  background-color: #f40612;
}
</style>
