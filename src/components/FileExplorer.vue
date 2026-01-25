<template>
  <section id="files" class="section-card">
    <h2 class="section-title">学习资料</h2>

    <p style="font-size:0.85rem;color:#888;margin-bottom:10px;">
      双击文件夹进入, 双击文件下载, pdf默认在浏览器中打开
    </p>

    <div class="w11-explorer">
      <div class="w11-toolbar">
        <button class="w11-back-btn" @click="goBack">
          <img src="https://img.icons8.com/material-outlined/48/left.png"
               style="width:20px;height:20px;">
        </button>

        <div class="w11-addr">
          <span class="crumb" @click="jumpTo(-1)">首页</span>
          <template v-for="(pathName, idx) in currentPath" :key="idx">
            <span style="color:#ccc"> > </span>
            <span class="crumb" @click="jumpTo(idx)">
              {{ pathName }}
            </span>
          </template>
        </div>
      </div>

      <div class="w11-content">
        <div class="w11-header">
          <div>名称</div>
          <div>说明</div>
        </div>

        <div v-for="(info, name) in currentData"
             :key="name"
             class="w11-item"
             :class="{ selected: selectedName === name }"
             @click="selectedName = name"
             @dblclick="handleDblClick(name, info)">
          <div class="w11-item-name">
            <img :src="getIcon(name, info)" style="width:20px;height:20px;">
            <span>{{ name }}</span>
          </div>
          <div class="w11-item-desc">
            {{ info.desc || '—' }}
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
import { ref, computed } from 'vue'

// 在 Vite 开发模式下，FILE_DB 挂在 window 上需要安全访问
const db = window.FILE_DB || {}

const currentPath = ref([])
const selectedName = ref('')

const iconMap = {
  folder: "https://img.icons8.com/fluency/48/folder-invoices.png",
  pdf: "https://img.icons8.com/?size=100&id=l0vjMqIboTRs&format=png&color=000000",
  ppt: "https://img.icons8.com/fluency/48/microsoft-powerpoint-2019.png",
  pptx: "https://img.icons8.com/fluency/48/microsoft-powerpoint-2019.png",
  xls: "https://img.icons8.com/fluency/48/microsoft-excel-2019.png",
  xlsx: "https://img.icons8.com/fluency/48/microsoft-excel-2019.png",
  doc: "https://img.icons8.com/fluency/48/microsoft-word-2019.png",
  docx: "https://img.icons8.com/fluency/48/microsoft-word-2019.png",
  zip: "https://img.icons8.com/?size=100&id=hwJDMFCJ6XVn&format=png&color=000000",
  mp3: "https://img.icons8.com/?size=100&id=zAzROOhkw_7N&format=png&color=000000",
  mkv: "https://img.icons8.com/?size=100&id=114331&format=png&color=000000",
  mp4: "https://img.icons8.com/?size=100&id=114331&format=png&color=000000",
  default: "https://img.icons8.com/fluency/48/file.png"
}

const currentData = computed(() => {
  let data = db
  currentPath.value.forEach(p => {
    if (data[p] && data[p].children) {
      data = data[p].children
    }
  })
  return data
})

function handleDblClick(name, info) {
  if (info.type === 'folder') {
    currentPath.value.push(name)
    selectedName.value = ''
  } else if (info.url) {
    window.open(info.url, '_blank')
  }
}

function goBack() {
  if (currentPath.value.length > 0) {
    currentPath.value.pop()
    selectedName.value = ''
  }
}

function jumpTo(index) {
  if (index === -1) {
    currentPath.value = []
  } else {
    currentPath.value = currentPath.value.slice(0, index + 1)
  }
  selectedName.value = ''
}

function getIcon(name, info) {
  if (info.type === 'folder') return iconMap.folder
  const ext = name.split('.').pop().toLowerCase()
  return iconMap[ext] || iconMap.default
}
</script>