<template>
  <div class="keyboard" @mousedown="clickButton" ref="keyBoard" @mouseup="removeClass">
    <ul class="top">
      <li v-for="(item, index) in keyButtons.top" :key="index" :class="item.class" v-html="item.content" :data-content="item.content" :data-real="item.real"></li>
    </ul>
    <ul class="keys">
      <li v-for="(item, index) in keyButtons.key" :key="index" :class="[item.class]" v-html="item.content" :data-content="item.content" :data-real="item.real"></li>
    </ul>
  </div>
</template>

<script>
import keyButtons from '@/utils/keyButtons.js';
// import { validSymbols } from '@/utils/calcWhite.js';
export default {
  name: 'KeyBoard',
  data() {
    return {
      keyButtons,
      allButtons: '',
    };
  },
  methods: {
    clickButton(e) {
      const buttonEl = e.target;
      const keyIsAvailable = buttonEl.dataset.real;
      if (keyIsAvailable) {
        this.sendInputKey(buttonEl, keyIsAvailable);
      }
    },
    getKeyDownContent(e) {
      const buttonContent = e.key;
      this.allButtons.some((buttonEl) => {
        let key = buttonEl.dataset.real;
        if (buttonContent === 'Delete' && key === 'Delete') {
          this.sendInputKey(buttonEl, 'Delete');
          return;
        }
        // 按下回车按照等号处理
        if (buttonContent === 'Enter' && key === '=') {
          this.sendInputKey(buttonEl, '=');
          return;
        }
        if (key == buttonContent) {
          this.sendInputKey(buttonEl, key);
        }
      });
    },
    removeClass() {
      this.allButtons.forEach((buttonEl) => {
        buttonEl.classList.remove('active');
      });
    },
    sendInputKey(el, key) {
      el.classList.add('active');
      this.$bus.$emit('input', key);
    },
  },
  mounted() {
    document.addEventListener('keydown', this.getKeyDownContent);
    document.addEventListener('keyup', this.removeClass);
    this.allButtons = this.$refs.keyBoard.querySelectorAll('li');
    this.allButtons = Array.from(this.allButtons);
  },
};
</script>

<style scoped>
.keyboard {
  margin-top: 30px;
  padding: 0 4px;
}
.keyboard li:hover {
  background-color: #b1b1b1;
  border: 1px solid #8b8989;
}

@keyframes buttonActive {
  from {
    background-color: #afafaf;
  }
  to {
    background-color: #a1a1a1;
  }
}

.top {
  display: flex;
  justify-content: space-around;
  text-align: center;
  font-size: 13px;
  font-weight: 600;
  line-height: 30px;
  margin-bottom: 5px;
}
.top li {
  margin: 0 2px;
  width: 100%;
}

.top .grayButton {
  color: #959595;
}
.top .grayButton:hover {
  border-color: transparent;
  background-color: transparent;
}

.keys {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 3px;
  text-align: center;
  font-size: 18px;
}
.keys li {
  --height: 65px;
  background-color: #d4d4d4;
  height: var(--height);
  line-height: var(--height);
}
/* 动态添加 */
.keys .number {
  background-color: #f1f1f1;
  font-weight: 600;
}
.keys .equal {
  background-color: #6f9fc3;
}
.keys .equal:hover {
  background-color: #388ccc;
}
.keyboard li.active {
  animation: buttonActive 1s linear 1 forwards;
}
.keys .equal.active {
  background-color: #0078d4;
  animation: none;
}
</style>