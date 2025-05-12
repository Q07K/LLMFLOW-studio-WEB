<template>
  <div class="vue-flow-container">
    <VueFlow :nodes="nodes" :edges="edges" :nodeTypes="nodeTypes" />
  </div>
</template>

<style scoped>
.vue-flow-container {
  width: 1000px;
  height: 1000px;
}

</style>

<script setup>
import { onMounted } from 'vue'; // 'computed' 제거
import { VueFlow, useVueFlow } from '@vue-flow/core';
import '@vue-flow/core/dist/style.css';
import '@vue-flow/core/dist/theme-default.css';
import DisplayNode from './components/nodes/DisplayNode/DisplayNode.vue'; // 경로가 올바른지 확인하세요.
import PromptNode from './components/nodes/PromptNode/PromptNode.vue';   // 경로가 올바른지 확인하세요.

// Vue Flow의 반응형 노드 및 엣지
const { nodes, edges, setNodes, setEdges } = useVueFlow();

// 노드 데이터 업데이트 함수
function updateNodeData(nodeId, newData) {
  const index = nodes.value.findIndex(node => node.id === nodeId);
  
  if (index !== -1) {
    const updatedNodes = [...nodes.value]; // 배열의 새로운 복사본 생성
    // 특정 노드 객체를 새로운 객체로 교체하여 반응성 트리거
    updatedNodes[index] = {
      ...updatedNodes[index],
      data: {
        ...updatedNodes[index].data,
        modelValue: newData // newData가 modelValue의 새 값이라고 가정
      }
    };
    
    setNodes(updatedNodes); // 변경된 노드 배열로 상태 업데이트
  } else {
    console.warn(`ID가 ${nodeId}인 노드를 찾을 수 없어 업데이트할 수 없습니다.`);
  }
}

// 노드 데이터 변경 이벤트 핸들러
function handleNodeDataChanged(event) {
  // event 객체가 { id: string, data: any } 형태라고 가정 (여기서 data는 새로운 modelValue)
  const { id, data } = event;
  updateNodeData(id, data);
}

// 초기 노드 설정
const initialNodes = [
  // {
  //   id: '1',
  //   position: { x: 100, y: 100 },
  //   type: 'display', 
  //   data: { jsonData: { message: '안녕하세요!', value: 123 } }
  // },
  // {
  //   id: '2',
  //   position: { x: 100, y: 400 },
  //   type: 'prompt_node',
  //   data: {
  //     modelValue: { role: 'system', context: 'Node 2의 초기 텍스트' },
  //   },
  //   events: {
  //     'update:modelValue': (newValue) => updateNodeData('2', newValue),
  //     'node-data-changed': handleNodeDataChanged
  //   }
  // },
  {
    id: '3',
    position: { x: 300, y: 400 },
    type: 'prompt_node',
    data: {
      modelValue: { role: 'user', context: 'Node 3의 초기 텍스트' },
      // id: '3'
    },
    events: {
      'update:modelValue': (newValue) => updateNodeData('3', newValue),
      'node-data-changed': handleNodeDataChanged
    }
  }
];

// 초기 엣지 설정
const initialEdges = [
  { id: 'e1-2', source: '1', target: '2', type: 'default' } 
];

// 노드 타입 등록
const nodeTypes = {
  display: DisplayNode,
  prompt_node: PromptNode
};

// Vue Flow 초기화
onMounted(() => {
  setNodes(initialNodes);
  setEdges(initialEdges);
});
</script>