<template>
  <div id="app">
    <VHeader />
    <CalculatorScreen />
    <KeyBoard />
  </div>
</template>

<script>
import VHeader from '@/components/VHeader.vue';
import CalculatorScreen from '@/components/CalculatorScreen.vue';
import KeyBoard from '@/components/KeyBoard.vue';
export default {
  name: 'App',
  components: {
    VHeader,
    CalculatorScreen,
    KeyBoard,
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
}
html,
body {
  width: 100%;
  background-color: black;
}
html,
body,
#app {
  height: 100%;
}
body {
  display: flex;
  align-items: center;
  justify-content: center;
}
ul {
  list-style: none;
}
li {
  border: 1px solid transparent;
}
#app {
  color: black;
  width: 400px;
  height: 661px;
  background-color: #b9b9b9;
  border-radius: 8px;
  overflow: hidden;
  cursor: default;
  user-select: none;
}
</style>
