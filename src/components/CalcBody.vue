<template>
  <div class="wrapper">
    <div class="calculator">
      <div class="show-block">
        
        <CalcResults :result="result"/>
        <CalcOperations :operations="operations"/>
      </div>
      <div class="buttons_block">
        <div class="block_1">
          <button class="btn clear" @click="clear">AC</button>
          <button class="btn delete" @click="deleteSymbol">x</button>
          <button class="btn" @click="click">/</button>
          <button class="btn" @click="click">*</button>
        </div>
        <div class="block_2">
          <button class="btn" @click="click">7</button>
          <button class="btn" @click="click">8</button>
          <button class="btn" @click="click">9</button>
          <button class="btn" @click="click">-</button>
        </div>
        <div class="block_3">
          <button class="btn" @click="click">4</button>
          <button class="btn" @click="click">5</button>
          <button class="btn" @click="click">6</button>
          <button class="btn plus" @click="click">+</button>
        </div>
        <div class="block_4">
          <button class="btn" @click="click">1</button>
          <button class="btn" @click="click">2</button>
          <button class="btn" @click="click">3</button>
          <button class="btn equal" @click="calculate">=</button>
        </div>
        <div class="block_5">
          <button class="btn null">0</button>
          <button class="btn">.</button>
        </div>
      </div>
    </div>
    <div class="calc-shadow"></div>
    <input type="checkbox" id="switch" />
    <label for="switch">Toggle</label>
  </div>
</template>

<script>
import CalcResults from "./CalcResults";
import CalcOperations from "./CalcOperations";

export default {
  name: "CalculatorBody",
  components: { CalcResults, CalcOperations },
  data() {
    return {
      result: "",
      operations: "",
    };
  },
  methods: {
    click(e) {
      this.result += e.target.textContent;
    },
    clear() {
      this.result = "";
    },
    deleteSymbol() {
      this.result = this.result.slice(0, -1);
    },
    calculate() {
      try {
        const operators = {
          "+": (a, b) => a + b,
          "-": (a, b) => a - b,
          "*": (a, b) => a * b,
          "/": (a, b) => a / b,
        };

        const tokens = [];
        let currentToken = "";

        for (let char of this.result) {
          if (operators[char]) {
            tokens.push(parseFloat(currentToken));
            tokens.push(char);
            currentToken = "";
          } else {
            currentToken += char;
          }
        }
        tokens.push(parseFloat(currentToken));

        let total = tokens.shift();
        while (tokens.length > 0) {
          const operator = tokens.shift();
          const operand = tokens.shift();
          total = operators[operator](total, operand);
        }

        this.result = total.toString();
        this.operations = this.result;
      } catch (error) {
        this.result = "Ошибка";
        this.operations = "";
      }
    },
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Nunito+Sans:opsz@6..12&family=Poppins:wght@500&display=swap");
.wrapper {
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: center;
  background: #daf0ff;
}
.calculator {
  width: 375px;
  height: 812px;
  border-radius: 39px;
  background: linear-gradient(223deg, #17181a 0%, #17181a 100%);
  margin: 20px;
  z-index: 2;
}
/* .calc-shadow {
  width: 362px;
  height: 568px;
  border-radius: 39px;
  background: rgba(0, 68, 115, 0.72);
  filter: blur(90.5px);
  position: absolute;
  left: 750px;
  top: 180px;
  z-index: -1;
} */
.show-block {
  width: 75%;
  display: flex;
  flex-direction: column;
  align-items: flex-end;
  justify-content: center;
  gap: 10px;
  width: 100%;
  height: 40%;
}
.clear,
.delete {
  color: #a5a5a5 !important;
  background: #616161 !important;
}
.btn {
  display: flex;
  width: 62px;
  height: 62px;
  padding: 6px 4px;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  border: 1px transparent;
  flex-shrink: 0;
  border-radius: 16px;
  background: #303136;
  color: #29a8ff;
  font-family: "Poppins";
  font-size: 28px;
  font-style: normal;
  font-weight: 500;
  line-height: normal;
  cursor: pointer;
}
.btn:hover {
  filter: brightness(120%);
  transition: 0.4s;
}

.btn:active {
  filter: brightness(80%);
  transition: 0.4s;
  scale: 0.95;
}

.block_1,
.block_2,
.block_3,
.block_4,
.block_5 {
  display: flex;
  flex-direction: row;
  width: 260px;
  height: 60px;
  gap: 25px;
  margin: 25px;
  flex-grow: 0;
}
.null {
  width: 150px;
}
.plus {
  height: 105px;
}
.equal {
  margin-top: 42px;
  height: 105px;
}
input[type="checkbox"] {
  height: 0 !important;
  width: 0 !important;
  visibility: hidden !important;
  z-index: 10;
}

label {
  cursor: pointer;
  text-indent: -9999px;
  width: 200px;
  height: 100px;
  background: grey;
  display: block;
  border-radius: 100px;
  position: relative;
}

label:after {
  content: "";
  position: absolute;
  top: 5px;
  left: 5px;
  width: 90px;
  height: 90px;
  background: #fff;
  border-radius: 90px;
  transition: 0.3s;
}

input:checked + label {
  background: #bada55; 
}

input:checked + label:after {
  left: calc(100% - 5px);
  transform: translateX(-100%);
}

label:active:after {
  width: 130px;
}

</style>
