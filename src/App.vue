<script setup>
import { ref } from 'vue';
import InputColor from './components/InputColor.vue'

import {
  passesWcagAaLargeText,
  passesWcagAa,
  passesWcagAaa,

} from 'passes-wcag';

import { Contrast } from "@smockle/contrast";

const backgroundColor = ref('#000000');
const foregroundColor = ref('#000000');
const contrastRatio = ref('1');
const aaPass = ref(false);
const aaaPass = ref(false);
const firstFourChars = ref();


function setColor1(text) {
  backgroundColor.value = text;
}

function setColor2(text) {
  foregroundColor.value = text;
}

function checkContrast(text1, text2) {
  contrastRatio.value = new Contrast(text1, text2);
  aaPass.value = passesWcagAa(text1, text2);
  aaaPass.value = passesWcagAaa(text1, text2);
  // Source: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/substring
  // I used this source to help me get only the first four characters of the Contrast Ratio String.
  firstFourChars.value = String(contrastRatio.value.value);
  firstFourChars.value = firstFourChars.value.substring(0, 4);
}


</script>

<template>
  <main>
    <div class="inputs">
      <div class="color-input" id="background-input">
        <h2 class="input-label">Background</h2>
        <InputColor v-on:colorSelected="setColor1" v-bind:color="backgroundColor" />
        <h3>{{ backgroundColor }}</h3>
      </div>
      <div class="color-input" id="foreground-input">
        <h2 class="input-label">Foreground</h2>
        <InputColor v-on:colorSelected="setColor2" v-bind:color="foregroundColor" />
        <h3>{{ foregroundColor }}</h3>
      </div>
    </div>
    <h2 class="preview-h2">Preview</h2>
    <!-- Source: Vue Documentation for In-Line Styling https://vuejs.org/guide/essentials/class-and-style
    I used this source to help my style my preview responsively. -->
    <div :style="{ background: backgroundColor }" class="preview">
      <h1 :style="{ color: foregroundColor }">Big Text</h1>
      <p :style="{ color: foregroundColor }">Normal Text</p>
    </div>
    <button v-on:click="checkContrast(backgroundColor, foregroundColor)">Check Contrast</button>
    <div>
      <h1 class="ratio" v-if="contrastRatio.value == null">Contrast Ratio: 1:1</h1>
      <h1 class="ratio" v-else-if="contrastRatio.value != null">Contrast Ratio: {{ firstFourChars }}:1</h1>

      <div class="tests">
        <!-- Source: https://symbl.cc/en/
        I used this reference to find the Hex codes for the checkmark and cross icons that I used in this div. -->
        <h2 class="pass" v-if="aaPass == true">Passes AA &#9989;</h2>
        <h2 class="fail" v-else-if="aaPass == false">Fails AA &#10060;</h2>
        <h2 class="pass" v-if="aaaPass == true">Passes AAA &#9989;</h2>
        <h2 class="fail" v-else-if="aaaPass == false">Fails AAA &#10060;</h2>
      </div>
    </div>
  </main>
</template>

<style >
#app {
  width: 75%;
  margin-top: 25px;
  color: black;
}

.preview {
  border-radius: 15px;
  padding: 50px;
  box-shadow: 5px 15px 20px #cdd1d4d6;

}

@media (max-width: 575px) {
  .input-label {
    font-size: 14px;
  }

  .preview-h2 {
    font-size: 14px;
  }
}


.ratio {
  font-weight: bold;
  text-align: center;
  margin: 20px;
  margin-top: 100px;
}

.preview h1 {
  font-weight: bold;
  text-align: center;
}

.preview p {
  font-weight: bold;
  text-align: center;
}

.fail {
  background-color: #ad0b0b;
  border-radius: 5px;
  display: inline-block;
  padding: 10px;
  margin: 10px;
  color: white;
  text-align: center;
}

.pass {
  background-color: rgb(10, 202, 42);
  border-radius: 5px;
  display: inline-block;
  padding: 10px;
  margin: 10px;
  color: white;
  text-align: center;
}

h2 {
  font-weight: bolder;
}

h3 {
  font-weight: bolder;
  text-transform: uppercase;
}


body {
  background-color: rgb(235, 238, 243);
  font-family: Arial, Helvetica, sans-serif;
  font-weight: bolder;
}

.preview-h2 {
  margin-top: 25px;
}

.color-input {
  display: flex;
  flex-direction: column;
  font-weight: bolder;
  width: 100%;
  padding: 20px;
  background-color: rgb(255, 255, 255);
  border-radius: 15px;
  margin: 10px;
  box-shadow: 5px 15px 20px #cdd1d4d6;

}

.inputs {
  display: flex;
  flex-direction: row;
  margin-left: auto;
  margin-right: auto;
}

button {
  background: #0091ff;
  margin-top: 20px;
  margin-bottom: 20px;
  padding: 15px;
  border-radius: 5px;
  border-style: none;
  color: white;
  font-weight: bold;
  font-size: 18px;
  float: right;

}

button:hover {
  background: #0a60f5;
  cursor: pointer;
}

#background-input {
  margin-left: 0px;
}

#foreground-input {
  margin-right: 0px;
}


.tests {
  display: flex;
  justify-content: center;
  margin-top: -20px;
}
</style>
