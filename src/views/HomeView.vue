<template>
  <main>
    <div class="type">
      <button v-for="item in typeList" :key="item" :class="type === item ? 'action-red' : '' "
          @click="typeHandle(item)">
          {{ item }}
      </button>
    </div>
    <Happy8 v-if="type === '快乐8'" />
  </main>
</template>

<script setup>
import { ref } from 'vue'
import Happy8 from '../components/Happy8.vue'

const typeList = ref(['快乐8', '双色球', '大乐透'])
const type = ref('快乐8')

function typeHandle(value) {
  type.value = value
}
</script>

<style scoped>
.type {
  padding: 18px 6px;
  display: flex;
  flex-wrap: wrap;
  gap: 18px;
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



