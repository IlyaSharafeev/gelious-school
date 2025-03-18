<template>
  <div :class="['calculator', isDark ? 'calculator--dark' : '']">
    <div class="calculator__button-switch-theme">
      <SwitchButton @click="toggleTheme"/>
    </div>
    <input v-model="display" class="calculator__display" readonly />
    <div class="calculator__buttons">
      <button v-for="btn in buttons" :key="btn" @click="onPress(btn)" :class="{'calculator__button--operator': isOperator(btn), 'calculator__button': true}">
        {{ btn }}
      </button>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import SwitchButton from "./SwitchButton.vue";

const display = ref(localStorage.getItem('lastResult') || '');
const isDark = ref(localStorage.getItem('theme') === 'dark');
const buttons = ['7', '8', '9', '/', '4', '5', '6', '*', '1', '2', '3', '=', '0', 'C'];

const calculate = (expression) => {
  try {
    const tokens = expression.match(/\d+|[/*]/g);
    if (!tokens) return 'Error';
    let result = parseInt(tokens[0], 10);
    for (let i = 1; i < tokens.length; i += 2) {
      const operator = tokens[i];
      const nextNum = parseInt(tokens[i + 1], 10);
      if (operator === '/' && nextNum === 0) return 'Error';
      result = operator === '*' ? result * nextNum : result / nextNum;
    }
    localStorage.setItem('lastResult', result.toString());
    return result.toString();
  } catch {
    return 'Error';
  }
};

const onPress = (btn) => {
  if (btn === '=') {
    display.value = calculate(display.value);
  } else if (btn === 'C') {
    display.value = '';
  } else {
    display.value += btn;
  }
};

const isOperator = (btn) => ['/', '*', '='].includes(btn);
const toggleTheme = () => {
  isDark.value = !isDark.value;
  localStorage.setItem('theme', isDark.value ? 'dark' : 'light');
};

const onKeyPress = (event) => {
  if (buttons.includes(event.key)) {
    onPress(event.key);
  } else if (event.key === 'Enter') {
    onPress('=');
  } else if (event.key === 'Backspace') {
    display.value = display.value.slice(0, -1);
  }
};

onMounted(() => {
  window.addEventListener('keydown', onKeyPress);
});
</script>

<style lang="scss" scoped>
.calculator {
  width: 100%;
  max-width: 320px;
  margin: auto;
  padding: 20px;
  border-radius: 10px;
  background: #f4f4f4;
  text-align: center;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
  transition: background 0.3s;

  &--dark {
    background: #333;
    color: white;
  }

  &__theme-toggle {
    background: none;
    border: none;
    font-size: 1.2rem;
    cursor: pointer;
    margin-bottom: 10px;
  }
  
  &__button-switch-theme {
    display: flex;
    justify-content: center;
  }

  &__display {
    width: 100%;
    padding: 10px;
    font-size: 2rem;
    text-align: right;
    border: none;
    background: white;
    color: #222;
    margin-bottom: 10px;
    border-radius: 5px;
  }

  &--dark &__display {
    background: #222;
    color: white;
  }

  &__buttons {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 10px;
  }

  &__button {
    padding: 15px;
    font-size: 1.5rem;
    border: none;
    border-radius: 5px;
    background: white;
    cursor: pointer;
    transition: background 0.3s, transform 0.1s;
    color: #444;

    &:hover {
      background: #ddd;
    }

    &:active {
      transform: scale(0.9);
      background: #ccc;
      box-shadow: 0 3px 5px rgba(0, 0, 0, 0.2);
    }
  }

  &--dark &__button {
    background: #444;
    color: white;

    &:hover {
      background: #555;
    }

    &:active {
      background: #666;
    }
  }

  &--dark &__button--operator {
    background: #ffcc00;
  }

  &__button--operator {
    background: #ffcc00;
  }
}
</style>