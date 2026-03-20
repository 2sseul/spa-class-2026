<script setup>
import { ref } from "vue";

const userInfo = ref({
  username: "",
  gender: "male",
  hobbies: [],
});
</script>

<template>
  <div>
    <h1>3주차 실습 시작</h1>
    <div class="container">
      <h2>실시간 회원가입 폼</h2>
      <div class="field">
        <label>이름: </label>
        <input
          type="text"
          v-model="userInfo.username"
          placeholder="이름을 입력하세요."
        />
      </div>
      <div class="field">
        <label>성별: </label>
        <label
          ><input
            type="radio"
            name="gender"
            value="male"
            v-model="userInfo.gender"
          />남성</label
        >
        <label
          ><input
            type="radio"
            name="gender"
            value="female"
            v-model="userInfo.gender"
          />여성</label
        >
      </div>
      <div class="field">
        <label>취미: </label>
        <label
          ><input
            type="checkbox"
            name="hobbies"
            value="코딩"
            v-model="userInfo.hobbies"
          />코딩</label
        >
        <label
          ><input
            type="checkbox"
            name="hobbies"
            value="영화"
            v-model="userInfo.hobbies"
          />영화</label
        >
        <label
          ><input
            type="checkbox"
            name="hobbies"
            value="게임"
            v-model="userInfo.hobbies"
          />게임</label
        >
      </div>
    </div>
    <hr />
    <h3>실시간 데이터 반영 결과:</h3>
    <pre class="result-box">{{ userInfo }}</pre>
  </div>
</template>

<style scoped>
.container {
  padding: 20px;
  font-family: sans-serif;
  max-width: 400px;
  margin: 0 auto;
  border: 1px solid #ccc;
  border-radius: 8px;
}

.field {
  margin-bottom: 20px;
}
label {
  margin-right: 10px;
  cursor: pointer;
}

.result-box {
  background: #282c34;
  color: #abb2bf;
  padding: 15px;
  border-radius: 8px;
  font-weight: bold;
}
</style>
