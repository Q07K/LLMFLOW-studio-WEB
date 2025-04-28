<template>
    <div :class="styles['node']">
        <div :class="styles['node-icon']"></div>
        <div :class="styles['vertical-line']"></div>
        <div>
            <div :class="styles['node-title']">PROMPT</div>
            <div :class="styles['node-subtitle']">{{ modelValue.role }}</div>
        </div>
        <div>.</div>
    </div>
    
<!--     
    <div :class="styles['prompt-node']">
      <div :class="styles['node-header']">
        <span>PROMPT</span>
        <span :class="styles['node-subtitle']">{{ modelValue.role }}</span>
        <div :class="styles['dropdown-container']">
          <div :class="styles['dropdown-button']" @click="toggleDropdown">
            <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" width="16" height="16" fill="currentColor"><path d="M7 10l5 5 5-5H7z"/></svg>
          </div>
          <div v-if="isDropdownOpen" :class="styles['dropdown-menu']">
            <div
              v-for="option in options"
              :key="option"
              :class="[styles['dropdown-item'], { [styles['selected']]: modelValue.role === option }]"
              @click="selectOption(option)"
            >{{ option }}</div>
          </div>
        </div>
      </div>
      <div :class="styles['node-content']">
        <label>TEXT</label>
        <textarea v-model="inputText" :class="styles['text-area']" />
      </div>
    </div> -->
  </template>
  
<script setup>
import { ref, watch, defineProps, defineEmits } from 'vue';
import styles from './PromptNode.module.css';

const props = defineProps({
  modelValue: {
    type: Object,
    default: () => ({ role: 'system', context: '' }),
  },
});

const emit = defineEmits(['update:modelValue', 'node-data-changed']);

const inputText = ref(props.modelValue.context);
// const isDropdownOpen = ref(false);
// const options = ref(['system', 'assistant', 'user']);

// const toggleDropdown = () => {
//   isDropdownOpen.value = !isDropdownOpen.value;
// };

// const selectOption = (option) => {
//   emit('update:modelValue', { ...props.modelValue, role: option, context: inputText.value });
//   emit('node-data-changed', { id: props.id, data: { role: option, context: inputText.value } });
//   isDropdownOpen.value = false;
// };

watch(inputText, (newValue) => {
  emit('update:modelValue', { ...props.modelValue, context: newValue });
  emit('node-data-changed', { id: props.id, data: { role: props.modelValue.role, context: newValue } });
});
</script>