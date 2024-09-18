<template>
  <div class="container">
    <div class="form-group">
      <label for="options">功能：</label>
      <select v-model="selectedOption" @change="Change" class="select-input">
        <option value="json">json格式化</option>
      </select>
    </div>
    
    <div class="form-group">
      <label for="inputText">输入内容：</label>
      <div class="input-wrapper">
        <textarea v-model="inputText" placeholder="请输入内容" rows="10" cols="50" class="text-area"></textarea>
        <button @click="copyText" class="btn-copy-icon">
          <i class="fas fa-copy"></i>
        </button>
      </div>
    </div>
    <button @click="ensure" class="btn-copy">确认</button>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import '@fortawesome/fontawesome-free/css/all.css'

let selectedOption = ref('json')
let inputText = ref('')

function Change() {
  console.log(selectedOption.value)
}

async function copyText() {
  // 将输入框中的内容复制到剪贴板
  navigator.clipboard.writeText(inputText.value)
    .then(() => {
      console.log('内容已复制到剪贴板');
    })
    .catch(err => {
      console.error('复制失败:', err);
    });
}

function ensure() {
  try {
    const parsed = JSON.parse(inputText.value)
    inputText.value = JSON.stringify(parsed, null, 2) // 格式化 JSON 字符串
  } catch (e) {
    console.error('无效的 JSON:', e)
  }
}
</script>

<style scoped>
.container {
  max-width: 600px;
  margin: 0 auto;
}

.form-group {
  margin-bottom: 20px;
}

.select-input {
  width: 100%;
  max-width: 500px;
  padding: 10px;
  font-size: 16px;
  border: 1px solid #ccc;
  border-radius: 4px;
  box-sizing: border-box;
}

.text-area {
  width: 100%;
  padding: 10px;
  font-size: 14px;
  border: 1px solid #ccc;
  border-radius: 4px;
  resize: vertical;
  max-height: 300px; /* 限制最大高度 */
  overflow-y: auto;  /* 添加垂直滚动条 */
  font-weight: bold;
  font-family: "Microsoft YaHei";
  color: #333; /* 设置字体颜色 */
}

.input-wrapper {
  position: relative;
  display: flex;
  align-items: center;
}

.btn-copy-icon {
  background: none;
  border: none;
  cursor: pointer;
  position: absolute;
  right: 10px;
  top: 10px;
  font-size: 20px;
  color: #007BFF;
}

.btn-copy-icon:hover {
  color: #0056b3;
}

.btn-copy, .btn-format {
  background: linear-gradient(90deg, #4CAF50, #66BB6A);
  color: white;
  border: none;
  padding: 12px 24px;
  font-size: 16px;
  cursor: pointer;
  border-radius: 25px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  transition: all 0.3s ease;
  margin-top: 10px;
  margin-right: 10px;
}

.btn-copy:hover, .btn-format:hover {
  background: linear-gradient(90deg, #45a049, #5fad5d);
  box-shadow: 0 6px 8px rgba(0, 0, 0, 0.2);
}

p {
  margin-top: 20px;
  font-size: 14px;
}
</style>