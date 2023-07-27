<script setup>
import { useToast } from 'vue-toast-notification'
import { ref, computed } from 'vue'
import wandIcon from './assets/wand.svg'

import 'vue-toast-notification/dist/theme-sugar.css'

const code = ref('')
const history = ref([])
const toast = useToast()

const animate = ref(false)
const replaced = computed(() => history.value[0] === code.value)

const sr = (input, output) => {
  code.value = code.value.replaceAll(input, output)
}

const r = (input, output) => {
  const regex = new RegExp(input, 'g')
  code.value = code.value.replace(regex, output)
}

const replaceCode = () => {
  r('  +', ' ')
  sr('}}', '.label}}')
  sr('{', '{{')
  sr('{{{', '{{')
  sr('{{{', '{{')
  sr('}', '}}')
  sr('}}}', '}}')
  sr('}}}', '}}')
  sr('{{%', '{%')
  sr('%}}', '%}')
  sr('______', 'false')
  r('\s*(inputlogicvariable1|inputlogicoperator|inputlogicvariable2|logicstartstring|logicendstring)="[^"]*"', '')
  r('\s*(inputlogicitem|inputlogiclist)="[^"]*"', '')
  r('<\s*span[^>]*>', '')
  r('</span>', '')
  r('<span([\S\s]*)class', '<span class')
  r('<\s*span[^>]*>', '')
  sr('devmode="false"', '')
  r('  +', ' ')

  const success = toast.success('Code replaced successfully! The good one is now in your clipboard.', {
    position: 'top'
  })

  if (code.value.includes('if ==')) {
    const warning = toast.warning('You have an "if ==" in your code. Please manually look for it and add the correct variables between "if" and "==".', {
      position: 'top'
    })
  }
}

const run = () => {
  if (replaced.value) return
  replaceCode()
  history.value.unshift(code.value)

  navigator.clipboard.writeText(code.value)
  animate.value = true
  setTimeout(() => animate.value = false, 1250)
}
</script>

<template>
  <textarea v-model="code" @focus="pasteOnFocus"></textarea>
  <div v-if="replaced && animate" class="decoration-done"></div>
  <button :disabled="replaced || !code" :class="{ 'button-done': replaced || !code }" @click="run"><img :src="wandIcon" width="24" height="24" /></button>
</template>

<style scoped>
textarea {
  width: 100%;
  min-height: 100vh;
  border: none;
  outline: none;
  resize: none;
  font-size: 1rem;
  font-weight: 600;
  font-family: 'Courier New', Courier, monospace;
  padding: 1rem;
  box-sizing: border-box;
  background-color: #f5f5f5;
  color: #333;
}

button {
  position: fixed;
  display: grid;
  place-content: center;
  bottom: 24px;
  right: 24px;
  width: 80px;
  height: 80px;
  border: none;
  outline: none;
  background-color: limegreen;
  border-radius: 50%;
  cursor: pointer;
  transition: all 0.4s ease-in-out;
  z-index: 2;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
  user-select: none;
}

.button-done {
  background-color: lightgrey;
  cursor: default;
}

.decoration-done {
  z-index: 1;
  position: absolute;
  width: 88px;
  height: 88px;
  border-radius: 50%;
  bottom: 12px;
  right: 12px;
  border-width: 0.5rem;
  border-style: solid;
  border-color: lightgrey transparent transparent transparent;
  animation: spin 1s ease-in-out forwards,
    shrink .25s 1s cubic-bezier(.77,-0.86,.39,1.07) forwards;
}

@keyframes spin {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(720deg);
  }
}

@keyframes shrink {
  0% {
    transform: scale(1);
  }
  100% {
    transform: scale(0);
  }
}
</style>
