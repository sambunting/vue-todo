<template>
  <input type="text" @change="inputUpdate"/>
</template>

<script setup>
  import { defineEmits } from 'vue';

  // Define a submit event so parent components can be notified of changes
  const emit = defineEmits(['submit']);

  /**
   * Method called when the input is updated
   *
   * @param {Event} event Event object
   */
  function inputUpdate(event) {
    const value = event.target.value;

    // Only submit the todo if the input field isn't empty, or isn't just a load of spaces
    if (value.trim().length > 0) {
      // Emit the submit event so it can be picked up by the parent component
      emit('submit', value)
    }
  }
</script>