<script setup>

import { ref } from 'vue';

const inputColor = ref();


</script>

<template>
  <main>
    <!-- Source: https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input/color
    I used this source to help me style and implement my color input. -->
    <input type="color" v-model="inputColor" @input="$emit('colorSelected', inputColor)" />
  </main>
</template>

<style scoped>
input {
  width: 100%;
  height: 150px;
  border: 0px;
  border-color: black;
  border-radius: 0px;
  border-style: none;
}
</style>
