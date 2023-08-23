<template>
  <div class="wrapper" :class="{ 'dark-mode': isDarkMode }">
    <div class="checkbox-wrapper-51">
      <input id="dark-mode-toggle" v-model="isDarkMode" type="checkbox" />
      <label class="toggle" for="dark-mode-toggle">
        <span>
          <svg viewBox="0 0 10 10" height="10px" width="10px">
            <path
              d="M5,1 L5,1 C2.790861,1 1,2.790861 1,5 L1,5 C1,7.209139 2.790861,9 5,9 L5,9 C7.209139,9 9,7.209139 9,5 L9,5 C9,2.790861 7.209139,1 5,1 L5,9 L5,1 Z"
            ></path>
          </svg>
        </span>
      </label>
    </div>
    <div class="calculator">
      <div class="show-block">
        <CalcOperations :operations="operations" />
        <CalcResults :result="result" />
      </div>
      <div class="buttons_block">
        <div
          v-for="(block, index) in buttonData"
          :key="index"
          :class="'block_' + (index + 1)"
        >
          <button
            v-for="button in block"
            :key="button.label"
            :class="['btn', button.class]"
            @click="handleButtonClick(button)"
          >
            {{ button.label }}
          </button>
        </div>
      </div>
    </div>
    <div class="calc-shadow"></div>
  </div>
</template>

<script>
import CalcOperations from "./CalcOperations";
import CalcResults from "./CalcResults";

export default {
  name: "CalculatorBody",
  components: { CalcOperations, CalcResults },
  data() {
    return {
      result: "",
      operations: "",
      isDarkMode: false,
      buttonData: [
        [
          { label: "AC", class: "clear", action: "clear" },
          { label: "\u{25C5}", class: "delete", action: "deleteSymbol" },
          { label: "/", class: "div", action: "click" },
          { label: "*", class: "mult", action: "click" },
        ],
        [
          { label: "7", class: "", action: "click" },
          { label: "8", class: "", action: "click" },
          { label: "9", class: "", action: "click" },
          { label: "-", class: "minus", action: "click" },
        ],
        [
          { label: "4", class: "", action: "click" },
          { label: "5", class: "", action: "click" },
          { label: "6", class: "", action: "click" },
          { label: "+", class: "plus", action: "click" },
        ],
        [
          { label: "1", class: "", action: "click" },
          { label: "2", class: "", action: "click" },
          { label: "3", class: "", action: "click" },
          { label: "=", class: "equal", action: "calculate" },
        ],
        [
          { label: "0", class: "null", action: "click" },
          { label: ".", class: "", action: "click" },
        ],
      ],
    };
  },
  methods: {
    click(e) {
      const clickedValue = e.target.textContent;
      if (clickedValue === "." && this.result.includes(".")) {
        return;
      }

      if (this.result === "0" && clickedValue !== ".") {
        this.result = clickedValue;
      } else if (this.result.length < 12) {
        this.result += clickedValue;
      }
    },
    clear() {
      this.result = "0";
      this.operations = "";
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
          "/": (a, b) => (b !== 0 ? a / b : "Ошибка: Деление на ноль!"),
        };

        const tokens = this.result.split(/([+\-*/])/).map((token) => {
          return isNaN(token) ? token : parseFloat(token);
        });

        let total = tokens.shift();
        let operationsString = total.toString();

        while (tokens.length > 0) {
          const operator = tokens.shift();
          const operand = tokens.shift();
          total = operators[operator](total, operand);
          operationsString += ` ${operator} ${operand}`;
        }

        if (typeof total !== "string") {
          this.result = `= ${total.toString()}`;
          this.operations = operationsString;
          if (this.result === "NaN") {
            this.result = "Ошибка: NaN";
          }
          if (this.result.length > 12) {
            this.result = this.result.substring(0, 12);
          }
        } else {
          this.result = total;
        }
      } catch (error) {
        this.result = "Ошибка";
        this.operations = "";
      }
    },
    handleButtonClick(button) {
      if (button.action === "click") {
        this.click({ target: { textContent: button.label } });
      } else if (button.action === "clear") {
        this.clear();
      } else if (button.action === "deleteSymbol") {
        this.deleteSymbol();
      } else if (button.action === "calculate") {
        this.calculate();
      }
    },
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Nunito+Sans:opsz@6..12&family=Poppins:wght@500&display=swap");
.wrapper {
  width: 100%;
  height: 100;
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: center;
  background: #daf0ff;
  user-select: none;
}
.wrapper.dark-mode {
  background: rgb(2, 0, 36);
  background: linear-gradient(
    90deg,
    rgba(2, 0, 36, 1) 0%,
    rgba(54, 129, 180, 1) 97%
  );
}
.wrapper.dark-mode .calculator {
  background: #f1f2f5;
  backdrop-filter: blur(51px);
}

.wrapper.dark-mode .btn {
  background-color: #ffffff;
}
.wrapper.dark-mode .mult,
.wrapper.dark-mode .div,
.wrapper.dark-mode .minus,
.wrapper.dark-mode .plus {
  background: #005db2;
}
.wrapper.dark-mode .equal {
  background: #19acff;
  box-shadow: -9px 13px 23px 0px rgba(0, 163, 255, 0.65),
    -3px 4px 11px 0px #b0dfff inset;
}

.calculator {
  width: 375px;
  height: 812px;
  border-radius: 39px;
  background: linear-gradient(223deg, #17181a 0%, #17181a 100%);
  margin: 20px;
  box-shadow: 25px 25px 63px -10px rgba(0, 107, 181, 0.63);
  z-index: 2;
}

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
.mult,
.div,
.minus,
.plus {
  background: #005db2;
}
.equal {
  background: #1991ff;
  color: #b2daff;
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
.checkbox-wrapper-51 input[type="checkbox"] {
  visibility: hidden;
  display: none;
}

.checkbox-wrapper-51 .toggle {
  position: relative;
  display: block;
  width: 42px;
  height: 24px;
  cursor: pointer;
  -webkit-tap-highlight-color: transparent;
  transform: translate3d(0, 0, 0);
}

.checkbox-wrapper-51 .toggle:before {
  content: "";
  position: relative;
  top: 1px;
  left: 1px;
  width: 40px;
  height: 22px;
  display: block;
  background: #c8ccd4;
  border-radius: 12px;
  transition: background 0.2s ease;
}

.checkbox-wrapper-51 .toggle span {
  position: absolute;
  top: 0;
  left: 0;
  width: 24px;
  height: 24px;
  display: block;
  background: #fff;
  border-radius: 50%;
  box-shadow: 0 2px 6px rgba(154, 153, 153, 0.75);
  transition: all 0.2s ease;
}

.checkbox-wrapper-51 .toggle span svg {
  margin: 7px;
  fill: none;
}

.checkbox-wrapper-51 .toggle span svg path {
  stroke: #c8ccd4;
  stroke-width: 2;
  stroke-linecap: round;
  stroke-linejoin: round;
  stroke-dasharray: 24;
  stroke-dashoffset: 0;
  transition: all 0.5s linear;
}

.checkbox-wrapper-51 input[type="checkbox"]:checked + .toggle:before {
  background: #1175c7;
}

.checkbox-wrapper-51 input[type="checkbox"]:checked + .toggle span {
  transform: translateX(18px);
}

.checkbox-wrapper-51 input[type="checkbox"]:checked + .toggle span path {
  stroke: #000000;
  stroke-dasharray: 25;
  stroke-dashoffset: 25;
}
.checkbox-wrapper-51 {
  position: absolute;
  left: 20%;
  top: 49vh;
}
</style>
