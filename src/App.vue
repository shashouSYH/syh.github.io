<template>
  <div id="app">
    <div class="bg-fixed-layer"></div>
    <div class="bg-overlay"></div>
    <div class="container">
      <Sidebar />
      <main>
        <IntroSection />
        <FileExplorer />
      </main>
    </div>
  </div>
</template>

<script setup>
import Sidebar from './components/Sidebar.vue'
import IntroSection from './components/IntroSection.vue'
import FileExplorer from './components/FileExplorer.vue'
</script>

<style>
@import "./style.css";
</style>