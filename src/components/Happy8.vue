<template>
  <div id="type-page">
    <div>
      <!-- <h3>玩法种类</h3> -->
      <div class="type">
        <button v-for="item in typeList" :key="item" :class="type === item ? 'action-red' : '' "
            @click="typeHandle(item)">
            选 {{ item }}
        </button>
      </div>
    </div>
    <div class="name-card">
      <div name="roll-animation">
        <div v-if="isRolling">
          <h3 style="margin: 18px 0 18px 0;">🎉 命运转动</h3>
          <div class="name-loading"></div>
        </div>
        <div v-if="currentStudent &&!isRolling">
          <h3 style="margin: 18px 0 18px 0;">🎉 财富密码</h3>
          <h2 style="color: #e74c3c; margin: 18px 0 18px 0;">{{ currentStudent }}</h2>
        </div>
      </div>
    </div>

    <div class="controls">
      <button class="action-blue" @click="roll" :disabled="isRolling">
          {{ isRolling? '命运转动' : '财富齿轮' }}
          <!-- <span v-if="isRolling" class="loading"></span> -->
      </button>
      <button class="action-red" @click="reset">重头再来</button>
      <button @click.self="showHistory">历史记录</button>
    </div>

    <!-- 数据列表 -->
    <div class="student-list">
      <div v-for="student in num" :key="student"
          :class="{ 'student-item': true, 'selected': selectedList.includes(student) }">
          {{ student }}
      </div>
    </div>
  </div>

  <div v-if="historyVisible" class="history" @click.self="closeDialog">
    <div class="dialog">
      <h3>历史选择记录</h3>
      <div class="item-list">
        <p v-if="!historyList.length">暂无历史记录</p>
        <p v-else>
        <h2 style="color: #e74c3c; margin: 18px 0 18px 0;" v-for="item in historyList" :key="item">{{ item }}</h2>
        </p>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, onMounted } from 'vue'

const num = ref([
  "1", "2", "3", "4", "5", "6", "7", "8", "9", "10",
  "11", "12", "13", "14", "15", "16", "17", "18", "19", "20",
  "21", "22", "23", "24", "25", "26", "27", "28", "29", "30",
  "31", "32", "33", "34", "35", "36", "37", "38", "39", "40",
  "41", "42", "43", "44", "45", "45", "47", "48", "49", "50",
  "51", "52", "53", "54", "55", "56", "57", "58", "59", "60",
  "61", "62", "63", "64", "65", "66", "67", "68", "69", "70",
  "71", "72", "73", "74", "75", "65", "77", "78", "79", "80",
])
const typeList = ref([1, 2, 3, 4, 5, 6, 7, 8, 9, 10])
const currentStudent = ref(null)
const type = ref(10)
const selectedList = ref([])
const isRolling = ref(false)
const historyList = ref(JSON.parse(localStorage.getItem('historyList')) || [])
const historyVisible = ref(false)

onMounted(() => {
  reset()
})

function getArr(arr) {
    return Array.from(new Set(arr))
}

function callBack(type) {
  let array = []
  for (let i = 0; i < type; i++) {
    const randomIndex = Math.floor(Math.random() * num.value.length);

    array.push(num.value[randomIndex])
  }

  array = getArr(array)

  console.log('type', array.length, type);

  if (array.length !== type) {
    callBack(type)
  }

  return array
}

function roll() {
  // alert('所有号码已选完，请重置！');

  isRolling.value = true;
  currentStudent.value = null;
  // 播放选择开始音效
  // const startSound = new Audio('https://www.soundjay.com/button/beep-07.wav');
  // startSound.play();

  setTimeout(() => {    
    // let array = []
    // for (let i = 0; i < type.value; i++) {
    //   const randomIndex = Math.floor(Math.random() * num.value.length);

    //   array.push(num.value[randomIndex])
    // }
    let array = callBack(type.value)
    array.sort(function(a, b) {
        return a - b; // 升序排序
    });

    array.forEach((item) => {
      if (currentStudent.value) {
        currentStudent.value = currentStudent.value + ' ' + item;
      } else {
        currentStudent.value = item;
      }
    })
    selectedList.value = array
    historyList.value.push(currentStudent.value);
    // 更新localStorage中的历史记录
    localStorage.setItem('historyList', JSON.stringify(historyList.value));
    isRolling.value = false;
    // 播放选择结束音效
    // const endSound = new Audio('https://www.soundjay.com/button/beep-06.wav');
    // endSound.play();
  }, 800);
}
function reset() {
  selectedList.value = [];
  currentStudent.value = null;
  isRolling.value = false;
  // 清空历史记录
  historyList.value = [];
  // 更新localStorage中的历史记录为空
  localStorage.setItem('historyList', JSON.stringify([]));
}
function typeHandle(value) {
  type.value = value
}
function showHistory() {
  historyVisible.value = true
}
function closeDialog() {
  historyVisible.value = false
}
</script>

<style scoped>
body {
  position: relative;
}

div {
  box-sizing: border-box;
}

/* 整体容器样式 */
#type-page {
  max-width: 568px;
  margin: 0 auto;
  /* padding: 20px; */
  font-family: "微软雅黑";
  text-align: center;
  color: #000;
}

/* 点名卡片样式 */
.name-card {
  min-height: 158px;
  background: #f8f9fa;
  border-radius: 15px;
  padding: 0 18px;
  overflow: hidden;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

/* 滚动动画 */
.roll-animation {
  animation: roll 0.8s ease-in-out;
}

@keyframes roll {
  0% {
      transform: rotate(-5deg);
  }

  50% {
      transform: rotate(5deg);
  }

  100% {
      transform: rotate(0);
  }
}

/* 控制按钮样式 */
.controls {
  margin-top: 18px;
  display: flex;
  justify-content: space-around;
  align-items: center;
}

.controls button {
  padding: 8px 18px;
  border: none;
  border-radius: 5px;
  font-size: 16px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

button:hover {
  opacity: 0.8;
}

button:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}

.action-blue {
  background-color: #3498db;
  color: white;
}

.action-red {
  background-color: #e74c3c;
  color: white;
}

/* loading特效样式 */
.loading {
  display: inline-block;
  width: 8px;
  height: 8px;
  border: 3px solid rgba(0, 0, 0, 0.1);
  border-top-color: #e74c3c;
  border-radius: 50%;
  animation: spin 1s ease-in-out infinite;
  margin-left: 6px;
}

@keyframes spin {
  to {
    transform: rotate(360deg);
  }
}

/* 点名框loading样式 */
.name-loading {
  display: inline-block;
  width: 28px;
  height: 28px;
  border: 5px solid rgba(0, 0, 0, 0.1);
  border-top-color: #e74c3c;
  border-radius: 50%;
  animation: spin 1s ease-in-out infinite;
  margin-top: 18px;
}

/* 历史记录样式 */
.history {
  width: 100%;
  height: 100%;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  background-color: rgba(0, 0, 0, 0.3);
  z-index: 999;
}

.history .dialog {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  background-color: white;
  width: 58%;
  max-width: 568px;
  height: 60%;
  padding: 8px;
  border-radius: 8px;
  text-align: center;
}

.history .dialog h3 {
  box-sizing: border-box;
  width: 100%;
  margin: 0;
  padding-top: 18px;
  padding-bottom: 18px;
  position: absolute;
  top: 0;
  left: 0;
  background-color: #fff;
  border-radius: 8px;
}

.history .dialog .item-list {
  width: 100%;
  height: 100%;
  padding-top: 40px;
  /* 内容超出可滑动 */
  overflow: scroll;
}

/* 列表样式 */
.student-list {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  margin-top: 18px;
  gap: 7px;
}

.student-item {
  box-sizing: border-box;
  display: flex;
  flex-basis: 8%;
  justify-content: center;
  align-items: center;
  height: 30px;
  line-height: 30px;
  border: 1px solid #ccc;
  border-radius: 5px;
}

.selected {
  background-color: rgba(255, 255, 0, 0.6);
}

.type {
  padding: 18px 6px;
  display: flex;
  flex-wrap: wrap;
  gap: 8px;
}

.type button {
  display: flex;
  flex-grow: 1;
  flex-shrink: 1;
  flex-basis: 16%;
  justify-content: center;
  align-items: center;
  padding: 6px 6px;
  border: none;
  border-radius: 5px;
  font-size: 16px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}
</style>