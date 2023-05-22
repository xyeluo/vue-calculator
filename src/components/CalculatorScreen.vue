<template>
  <div class="screen">
    <div class="expression">{{ expressionShow }}</div>
    <div class="input_result">
      <div>{{ input.length > 0 ? input : result }}</div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'CalculatorScreen',
  data() {
    return {
      // 显示屏展示表达式（用户看到的）
      expressionShow: '',
      // 临时表达式，将转换后的表达式与展示表达式同步（中间层）
      tempExpression: '',
      // 实际运算的表达式
      expression: '',
      input: '',
      isInputReset: false,
      isExpressionReset: false,
    };
  },
  computed: {
    result() {
      const lastChar = this.expression.slice(this.expression.length - 1);
      if (this.operator.includes(lastChar)) {
        return 0;
      }
      return eval(this.expression) ?? 0;
    },
  },
  watch: {
    tempExpression(nValue) {
      if (this.operator.includes(nValue.slice(0, 1))) {
        return;
      }
      let changedOperator = [...nValue].reduce((pre, curr) => {
        return pre + (this.operatorMap[curr] ?? curr);
      });
      this.expressionShow = this.expressionShow + changedOperator;

      if (nValue.slice(nValue.length - 1) === '=') {
        this.input = '';
        this.isExpressionReset = true;
        nValue = nValue.slice(0, nValue.length - 1);
      }
      this.expression = this.expression + nValue;
    },
  },
  methods: {
    handleCalc(params) {
      if (params === 'Delete') {
        this.deleteAll();
        return;
      }
      if (params === 'Backspace') {
        this.backspaceInput();
        return;
      }
      // 当出现四则运算符时把当前input赋值给tempExpression
      if (this.operator.includes(params)) {
        this.tempExpression = this.input + params;
        this.isInputReset = true;
        return;
      }
      if (this.isExpressionReset) {
        this.expression = '';
        this.expressionShow = '';
        this.isExpressionReset = false;
      }
      if (this.isInputReset || this.isExpressionReset) {
        this.input = '';
        this.isInputReset = false;
      }
      this.input += params;
      this.parseInput();
    },
    parseInput() {
      // 去掉所有开头的零
      let isZeroStart;
      do {
        isZeroStart = false;
        if (this.input.slice(0, 1) === '0') {
          isZeroStart = true;
          this.input = this.input.slice(1);
        }
      } while (isZeroStart);
      // 小数点开头，去除多余小数点后前面补上'0.'
      if (this.input.match(/^\./)) {
        this.input = '0.' + this.input.slice(1).replace(/^\./g, '');
      }
    },
    backspaceInput() {
      if (!this.input.length) {
        this.input = '';
        return;
      }
      this.input = this.input.slice(0, this.input.length - 1);
    },
    deleteAll() {
      this.expression = '';
      this.expressionShow = '';
      this.input = '';
      // this.tempExpression = '';
    },
  },
  mounted() {
    this.$bus.$on('input', this.handleCalc);
  },
  created() {
    this.operator = ['+', '-', '*', '/', '='];
    this.operatorMap = { '*': '×', '/': '÷' };
  },
};
</script>

<style scoped>
.screen {
  --base_height: 30px;
  display: flex;
  padding: 0 15px;
  flex-direction: column;
  text-align: right;
}
.expression,
.input_result {
  width: 100%;
}
.expression {
  height: var(--base_height);
  line-height: var(--base_height);
}
.input_result {
  font-size: calc(var(--base_height) + 5px);
  font-weight: 800;
  height: calc(var(--base_height) * 2);
  line-height: calc(var(--base_height) * 2);
}
</style>