<template>
  <div @click="toggleComplete" :class="`${completed ? 'completed' : ''}`">
    {{text}}
  </div>
</template>

<script setup>
  import { ref, defineEmits } from 'vue';

  /**
   * Define props for the component
   */
  const props = defineProps({
    /**
     * The todo text
     */
    text: { type: String },
    /**
     * Bool on if the task/todo has been completed or not
     */
    completed: { type: Boolean }
  })
  
  // Define a submit event so parent components can be notified of changes
  const emit = defineEmits(['update'])

  // Set the initial value to the prop value
  const completed = ref(props.completed);

  /**
   * Method called to toggle the complete status and notify the parent component
   */
  function toggleComplete() {
    completed.value = !completed.value;

    emit('update', props.text, completed.value);
  }
</script>

<style scoped>
  div {
    cursor: pointer;
  }

  div.completed {
    text-decoration: line-through;
  }
</style>