<template>
  <div class="confirm" v-if="visible" @click.self="emit('close')">
    <div class="dialog">
      <div class="X-box" @click="emit('close')">
        <div class="close-button">x</div>
      </div>
      <div class="content">
        <slot></slot>
      </div>
      <div class="operation">
        <button style="margin-right: 8px ;" @click="emit('close')">取消</button>
        <button class="action-blue" @click="confirm">确定</button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
const emit = defineEmits(['close', 'confirm'])

const { visible } = defineProps({
  visible: {
    type: Boolean,
    default: false
  }
})

function confirm() {
  emit('confirm')
}


</script>

<style scoped>
.confirm {
  width: 100%;
  height: 100%;
  position: fixed;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  background-color: rgba(0, 0, 0, 0.3);
  z-index: 999;
  color: black;
}

.confirm .dialog {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  background-color: white;
  width: 58%;
  max-width: 420px;
  padding: 18px;
  border-radius: 8px;
}

.confirm .dialog .content {
  padding: 16px 0;
  /* padding: 8px 0 16px 0; */
}

.confirm .dialog .operation {
  padding-top: 8px;
  display: flex;
  justify-content: end;
}

.confirm .dialog .X-box {
  position: relative;
}

.confirm .dialog .X-box .close-button {
  position: absolute;
  top: -10px;
  right: -10px;
  width: 16px;
  height: 16px;
  font-size: 20px;
  display: flex;
  justify-content: center;
  align-items: center;
  cursor: pointer;
}


/* button {
  margin: 0 8px;
  padding: 8px 18px;
  border: none;
  border-radius: 5px;
  font-size: 16px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

button:hover {
  opacity: 0.8;
  color: #aaa;
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
} */
</style>