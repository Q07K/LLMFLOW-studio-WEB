<template>
  <div class="vue-flow-container">
    <VueFlow :nodes="nodes" :edges="edges" :nodeTypes="nodeTypes" />
  </div>
</template>

<style scoped>
.vue-flow-container {
  width: 100vw; /* 뷰포트 너비의 100% */
  height: 100vh; /* 뷰포트 높이의 100% */
}
</style>

<script setup>
import { ref } from 'vue';
import { VueFlow } from '@vue-flow/core';
import '@vue-flow/core/dist/style.css';
import '@vue-flow/core/dist/theme-default.css';
import DisplayNode from './components/nodes/DisplayNode/DisplayNode.vue'
import PromptNode from './components/nodes/PromptNode/PromptNode.vue'

const nodes = ref([
  { id: '1', position: { x: 50, y: 50 }, data: { label: '노드 1' } },
  { id: '2', position: { x: 100, y: 100 }, data: { label: '노드 2' } },
  { id: '3', position: { x: 150, y: 150 }, type: 'display', data:  { jsonData: { message: '안녕하세요!', value: 123 } },},
  { id: '4', position: { x: 250, y: 300 }, type: 'prompt_node', data:  { jsonData: { message: '안녕하세요!', value: 123 } },}
]);

const edges = ref([
  { id: 'e1-1', source: '1', target: '2' },
  { id: 'e1-2', source: '2', target: '3' },
]);

const nodeTypes = ref({
  display: DisplayNode,
  prompt_node: PromptNode
});
</script>