<template>
  <div :style="styleVars" class="vue-input-calculator" tabindex="0" @focus="handleFocus"
    @blur="handleBlur">
    <transition name="slide">
      <div
        :class="
          place === 'in-place' ? 'calc-wrapper-inplace' : 'calc-wrapper-fixed'
        "
        v-if="show"
      >
        <div class="calculator" >
          <!-- {{enableKeyboard}} {{enableKeyboard}} -->
          <div class="calculator-logs" ref="historyLog" v-if="isHistoryLogs">
            <span
              v-for="(log, index) in logs"
              :key="index"
              @click="logToValue(log)"
              >{{ log }}</span
            >
          </div>
          <input
            readonly
            type="text"
            class="calculator-input"
            v-model="expresion"
          />
          <div class="calculator-row">
            <div class="calculator-col">
              <button class="calculator-btn gray action" @click="memory('mc')">
                MC
              </button>
            </div>
            <div class="calculator-col">
              <button class="calculator-btn gray action" @click="memory('mr')">
                MR
              </button>
            </div>
            <div class="calculator-col">
              <button class="calculator-btn gray action" @click="memory('m+')">
                M+
              </button>
            </div>
            <div class="calculator-col">
              <button class="calculator-btn gray action" @click="memory('m-')">
                M-
              </button>
            </div>
            <div class="calculator-col">
              <button class="calculator-btn gray action" @click="memory('ms')">
                MS
              </button>
            </div>
          </div>
          <div class="calculator-row">
            <div class="calculator-col">
              <button
                class="calculator-btn accent action"
                @click="touchHandler('^')"
              >
                x<sup style="font-size: 16px">y</sup>
              </button>
            </div>
            <div class="calculator-col">
              <button
                class="calculator-btn accent action"
                @click="touchHandler('%')"
              >
                %
              </button>
            </div>
            <div class="calculator-col">
              <button
                class="calculator-btn accent action"
                @click="touchHandler('1/')"
              >
                1/x
              </button>
            </div>
            <div class="calculator-col">
              <button
                class="calculator-btn gray action"
                @click="deleteLastChar"
              >
                &rsaquo;
              </button>
            </div>
            <!-- <div class="calculator-col">
              <button
                class="calculator-btn accent action"
                @click="touchHandler('x2')"
              >
                x<sup style="font-size: 16px">2</sup>
              </button>
            </div> -->
            <!-- <div class="calculator-col">
              <button
                class="calculator-btn accent action"
                @click="touchHandler('√')"
              >
                √
              </button>
            </div> -->
          </div>
          <!-- <div class="calculator-row">
            
            
            <div class="calculator-col">
              <button
                class="calculator-btn accent action"
                @click="touchHandler('√')"
              >
                √
              </button>
            </div>
          </div> -->
          <div class="calculator-row">
            <div class="calculator-col">
              <button class="calculator-btn gray action" @click="clear()">
                c
              </button>
            </div>
            <div class="calculator-col">
              <button class="calculator-btn accent action" @click="calcLog()">
                log
              </button>
            </div>
            <div class="calculator-col">
              <button
                class="calculator-btn accent action"
                @click="calcSqrt('√')"
              >
                √
              </button>
            </div>

            <!-- <div class="calculator-col">
              <button
                class="calculator-btn accent action"
                @click="touchHandler('(')"
              >
                (
              </button>
            </div>
            <div class="calculator-col">
              <button
                class="calculator-btn accent action"
                @click="touchHandler(')')"
              >
                )
              </button>
            </div> -->

            <div class="calculator-col">
              <button
                class="calculator-btn accent action"
                @click="touchHandler('/')"
              >
                /
              </button>
            </div>
          </div>

          <div class="calculator-row">
            <div class="calculator-col">
              <button class="calculator-btn" @click="touchHandler('7')">
                7
              </button>
            </div>
            <div class="calculator-col">
              <button class="calculator-btn" @click="touchHandler('8')">
                8
              </button>
            </div>
            <div class="calculator-col">
              <button class="calculator-btn" @click="touchHandler('9')">
                9
              </button>
            </div>
            <div class="calculator-col">
              <button
                class="calculator-btn accent action"
                @click="touchHandler('*')"
              >
                *
              </button>
            </div>
          </div>
          <div class="calculator-row">
            <div class="calculator-col">
              <button class="calculator-btn" @click="touchHandler('4')">
                4
              </button>
            </div>
            <div class="calculator-col">
              <button class="calculator-btn" @click="touchHandler('5')">
                5
              </button>
            </div>
            <div class="calculator-col">
              <button class="calculator-btn" @click="touchHandler('6')">
                6
              </button>
            </div>
            <div class="calculator-col">
              <button
                class="calculator-btn accent action"
                @click="touchHandler('-')"
              >
                -
              </button>
            </div>
          </div>
          <div class="calculator-row">
            <div class="calculator-col">
              <button class="calculator-btn" @click="touchHandler('1')">
                1
              </button>
            </div>
            <div class="calculator-col">
              <button class="calculator-btn" @click="touchHandler('2')">
                2
              </button>
            </div>
            <div class="calculator-col">
              <button class="calculator-btn" @click="touchHandler('3')">
                3
              </button>
            </div>
            <div class="calculator-col">
              <button
                class="calculator-btn accent action"
                @click="touchHandler('+')"
              >
                +
              </button>
            </div>
          </div>
          <div class="calculator-row">
            <div class="calculator-col wide">
              <button class="calculator-btn" @click="touchHandler('0')">
                0
              </button>
            </div>
            <div class="calculator-col">
              <button class="calculator-btn action" @click="touchHandler('.')">
                .
              </button>
            </div>
            <div class="calculator-col">
              <button
                v-if="isResult"
                class="calculator-btn success action"
                @click="applyResult"
              >
                ок
              </button>
              <button
                v-else
                class="calculator-btn accent action"
                @click="touchHandler('=')"
              >
                =
              </button>
            </div>
          </div>
        </div>
      </div>
    </transition>
    <transition name="opacity">
      <div
        v-if="show"
        @click="persistent ? '' : hideInterface()"
        class="backdrop"
      ></div>
    </transition>
  </div>
</template>

<script>
import { create, all } from "mathjs";
const math = create(all);

export default {
  name: "CalculatorInterface",
  emits: ["hide", "update:modelValue"],
  props: [
    "modelValue",
    "enableKeyboard",
    "zIndex",
    "textColor",
    "triggerWrapperClass",
    "bgColor",
    "eventButtonsBgColor",
    "numberButtonsBgColor",
    "actionButtonsBgColor",
    "actionSuccessButtonBgColor",
    "isHistoryLogs",
    "autoApply",
    "persistent",
    "floatResultFixedCount",
    "show",
    "place",
  ],
  data() {
    return {
      showStyles: false,
      isResult: false, // последнее действие было расчётом результата
      lastEventValueType: null, // тип данных последнего нажатия - для блокировки нескольких знаков действия подряд
      expresion: "0",
      logs: [],
      error: false,
      memoryData: {
        saved: false,
        value: 0,
      },
    };
  },
  mounted() {
    if (this.enableKeyboard) {
      //console.log('mounteds add hiiv!');
      document.addEventListener("keydown", this.keyboardHandler);
    }
  },
  methods: {
    handleFocus() {
      //this.enableKeyboard=true;
      //console.log('Div focused! s');
      // Add your focus event handling logic here
    },
    handleBlur() {
      //this.$emit('enable', false)
      //this.enableKeyboard=false;
      //console.log('Div blurred!');
      // Add your blur event handling logic here
    },
    keyboardHandler(event) {
      //console.log(event);
      let allowValue = event.key.match(/[0-9%/*\-+=.,]|Backspace|Enter/);
      let input = allowValue ? allowValue.input : null;
      //console.log("input", input);
      if (input) {
        //console.log(input)
        // адаптация ключа клавиши под общий стандарт
        // заменим запятую на точку для универсальности при разных раскладках
        if (input === ",") input = ".";
        if (input === "Enter") input = "=";
        if (input === "Backspace") return this.deleteLastChar();

        this.prepareInput(input);
      }
    },
    touchHandler(value) {
      this.prepareInput(value);
    },
    prepareInput(value) {
      let inputIsAction = Number.isNaN(Number.parseInt(value));
      // последний симовол выражения
      let lastSimbol = this.expresion.slice(-1);
      let lastSimbolIsAction = Number.isNaN(Number.parseInt(lastSimbol));
      if (lastSimbol == ")" || lastSimbol == "(") {
        lastSimbolIsAction = false;
      }
      // console.log({
      //   value,
      //   expresion: this.expresion,
      //   inputIsAction,
      //   lastSimbolIsAction,
      //   lastSimbol,
      // });

      // если выражение не изменялось и пытаются ввести 0 - игнорим
      if (this.expresion === "0") {
        if (value === "0") {
          // значение обнулено - значит ничего не нужно делать
          return;
        } else if (!inputIsAction) {
          return (this.expresion = value);
        }
      }

      switch (true) {
        case inputIsAction &&
          lastSimbolIsAction &&
          value !== "." &&
          value !== "=" &&
          value !== ")":
          //console.log("sw 1");
          //console.error(1);
          // провожу замену последнего символа
          this.expresion = this.expresion.slice(0, -1) + value;
          break;
        case inputIsAction && value === "=":
          //console.log("sw 2");
          //console.error(2, this.isResult);
          // проверю наличие незавершенного выражения - мат действия в конце
          // при наличии удаляю знак c конца и провожу вычисления
          if (lastSimbolIsAction) {
            //console.error(21);
            //console.log("sw 3");
            this.expresion = this.expresion.slice(0, -1);
          }
          if (this.isResult) {
            //console.log("sw 4");
            this.applyResult();
          } else {
            //console.log("sw 5");
            this.calculate(); // расчитываю результат по выражению
          }
          break;
        case inputIsAction && value === ".":
          //console.log("sw 6");
          //console.error(3);
          // если знак . присутствует - игнорю
          if (
            Array.isArray(this.expresion.split(/[/*\-+]/)) &&
            (this.expresion
              .split(/[/*\-+]/)
              .slice(-1)[0]
              .indexOf(".") > -1 ||
              lastSimbolIsAction)
          ) {
            break;
          } else {
            this.isResult = false;
            this.expresion += value;
          }
          break;
        default:
          this.isResult = false;
          this.expresion += value;
          //console.error('prepareInput', value);
          break;
      }
    },
    memory(val) {
      let resultNotNumber = Number.isNaN(Number.parseInt(this.expresion));
      switch (val) {
        case "mc":
          this.memoryData.saved = false;
          this.memoryData.value = 0;
          break;
        case "m+":
          if (!resultNotNumber) {
            this.memoryData.saved = true;
            this.memoryData.value =
              this.memoryData.value + Number.parseInt(this.expresion);
            if (this.autoApply) {
              this.applyResult();
            }
          }
          break;
        case "m-":
          if (!resultNotNumber) {
            this.memoryData.saved = true;
            this.memoryData.value =
              this.memoryData.value - Number.parseInt(this.expresion);
            if (this.autoApply) {
              this.applyResult();
            }
          }
          break;
        case "ms":
          if (!resultNotNumber) {
            this.memoryData.saved = true;
            this.memoryData.value = this.expresion;
            if (this.autoApply) {
              this.applyResult();
            }
          }
          break;
        case "mr":
          if (this.memoryData.saved) {
            //this.clear();
            this.expresion =
              this.expresion == "0"
                ? this.memoryData.value + ""
                : this.expresion + this.memoryData.value + "";
            //this.prepareInput( this.memoryData.value);
          }

          break;
      }
    },
    calcSqrt() {
      let log = this.expresion;
      //console.log(this.expresion);
      let resultNotNumber = Number.isNaN(Number.parseInt(this.expresion));
      //console.log(resultNotNumber);
      if (!resultNotNumber) {
        //this.expresion = 'log()'
        let result = math.sqrt(this.expresion);
        log = `sqrt(${this.expresion})`;
        this.expresion = result.toString();
        this.isResult = true;
        this.logs.push(log + `=${result}`);

        this.$nextTick(() => {
          if (this.$refs.historyLog) {
            this.$refs.historyLog.scrollTop =
              this.$refs.historyLog.scrollHeight;
          }
        });

        if (this.autoApply) {
          this.applyResult();
        }
      }
    },
    calcLog() {
      let log = this.expresion;
      console.log(this.expresion);
      let resultNotNumber = Number.isNaN(Number.parseInt(this.expresion));
      console.log(resultNotNumber);
      if (!resultNotNumber) {
        //this.expresion = 'log()'
        let result = math.log(this.expresion, 10);
        log = `log(${this.expresion})`;
        this.expresion = result.toString();
        this.isResult = true;
        this.logs.push(log + `=${result}`);

        this.$nextTick(() => {
          if (this.$refs.historyLog) {
            this.$refs.historyLog.scrollTop =
              this.$refs.historyLog.scrollHeight;
          }
        });

        if (this.autoApply) {
          this.applyResult();
        }
      }
    },
    calculate() {
      //console.log("calc started", math);
      let log = this.expresion;
      //console.log("calc started 1");
      // Разобрать выражение на части спарсить числа и собрать снова
      // используя parseFloat - чтобы не было ошибок с 5+09 = ERROR
      let regex = /[/*\-+%^()]/g;
      let actions = this.expresion.match(regex);
      if (!actions) {
        return (this.isResult = true);
      }
      //console.log("calc started 2");
      //let numbers = this.expresion.split(regex);
      // this.expresion = numbers.reduce((str, currNum, index) => {
      //   // если выражение начинается с отрицательного значения "-"
      //   // то игнорю пустое текущее значение и поставим только знак действия
      //   if (currNum) {
      //     str = str + parseFloat(currNum) + (actions[index] || '');
      //   } else {
      //     str = str + (actions[index] || '');
      //   }
      //   return str
      // }, "");

      // console.error({
      //   actions,
      //   numbers,
      //   valideExpresion: this.expresion
      // });

      // пробуем выполнить операцию
      try {
        //console.log("calc started 3", this.expresion);
        let result = math.evaluate(this.expresion);
        //console.log({ result });
        // console.warn({result},Number.isInteger(result));
        //this.expresion = Number.isInteger(result) ? result.toString() : parseFloat(parseFloat(result).toFixed(this.floatResultFixedCount)).toString();
        this.expresion = result.toString();
        this.isResult = true;
        this.logs.push(log + `=${result}`);
      } catch (ex) {
        //console.log(ex);

        // alert('Error eval: ' + this.expresion);
        this.error = true;
      }

      // прокрутим лог к последнему действию
      this.$nextTick(() => {
        if (this.$refs.historyLog) {
          this.$refs.historyLog.scrollTop = this.$refs.historyLog.scrollHeight;
        }
      });

      if (this.autoApply) {
        this.applyResult();
      }
    },
    applyResult() {
      let resultNotNumber = Number.isNaN(Number.parseInt(this.expresion));
      this.$emit("input", resultNotNumber ? "0" : this.expresion);
      this.hideInterface();
    },
    hideInterface() {
      this.$emit("hide");
      // скрою стили только после отыгрыша анимации
      // TODO плохо! Нужно придумать выход!
      setTimeout(() => {
        this.showStyles = false;
      }, 500);
    },
    clear() {
      this.expresion = "0";
      this.isResult = false;
    },
    deleteLastChar() {
      // console.log('deleteLastChar ', this.expresion);
      if (this.expresion !== "0") {
        this.expresion = this.expresion.slice(0, -1);
        if (!this.expresion) {
          this.expresion = "0";
        }
      }
    },
    logToValue(log) {
      if (log) {
        this.expresion = log.replace(/=.*/, "");
        this.isResult = false;
      }
    },
  },
  computed: {
    styleVars() {
      return {
        "--vue-input-calculator-text-color": this.textColor,
        "--vue-input-calculator-bg-color": this.bgColor,
        "--vue-input-calculator-event-btn-bg-color": this.eventButtonsBgColor,
        "--vue-input-calculator-number-btn-bg-color": this.numberButtonsBgColor,
        "--vue-input-calculator-action-btn-bg-color": this.actionButtonsBgColor,
        "--vue-input-calculator-action-success-btn-bg-color":
          this.actionSuccessButtonBgColor,
        ...(this.showStyles && {
          display: "flex",
          flexDirection: "column",
          justifyContent: "flex-end",
          alignItems: "center",
          height: "100%",
          width: "100%",
          position: this.place === "in-place" ? "" : "absolute",
          top: 0,
          left: 0,
          zIndex: this.zIndex,
        }),
      };
    },
  },
  watch: {
    enableKeyboard(newValue) {
      console.log("what is this", newValue)
      if (newValue) {
        //document.body.style.overflow = "hidden";
        //this.expresion = this.modelValue.toString();
        //this.showStyles = true;
        // если клавиатура не используется - не множим слушателей
        if (this.enableKeyboard) { 
          console.log("add hiiv enablees", this.enableKeyboard)
          document.addEventListener("keydown", this.keyboardHandler);
        }
      } else {
        //document.body.style.overflow = "initial";
        //if (this.enableKeyboard) {
          console.log("remove hiiv enablees")
          document.removeEventListener("keydown", this.keyboardHandler);
        //}
      }
    },
    show(newValue) {
      if (newValue) {
        document.body.style.overflow = "hidden";
        this.expresion = this.modelValue.toString();
        this.showStyles = true;
        // если клавиатура не используется - не множим слушателей
        if (this.enableKeyboard) {
          console.log("add hiiv showws")
          document.addEventListener("keydown", this.keyboardHandler);
        }
      } else {
        document.body.style.overflow = "initial";
        if (this.enableKeyboard) {
          console.log("remove hiiv showws")
          document.removeEventListener("keydown", this.keyboardHandler);
        }
      }
    },
  },
};
</script>

<style lang="scss" scoped>
*,
::after,
::before {
  box-sizing: border-box;
}

.vue-input-calculator {
  z-index: 0;
  -webkit-touch-callout: none; /* iOS Safari */
  -webkit-user-select: none; /* Chrome/Safari/Opera */
  -khtml-user-select: none; /* Konqueror */
  -moz-user-select: none; /* Firefox */
  -ms-user-select: none; /* Internet Explorer/Edge */
  user-select: none; /* Non-prefixed version, currently not supported by any browser */

  // обёртка калькулятора
  .calc-wrapper-fixed {
    padding: 0;
    position: fixed;
    bottom: 0;
    z-index: 1;
  }
  .calc-wrapper-inplace {
    padding: 0;
    bottom: 0;
    z-index: 1;
  }

  // калькулятор
  .calculator {
    width: 100%;
    display: flex;
    margin: 0 auto;
    flex-direction: column;
    max-width: 720px;
    min-width: 320px;
    background-color: var(--vue-input-calculator-bg-color);
    box-shadow: 0 0 0 1px var(--vue-input-calculator-bg-color);

    .calculator-logs {
      padding: 0 0.8rem 0 0.8rem;
      max-height: 100px;
      display: flex;
      position: relative;
      overflow: auto;
      align-items: flex-end;
      flex-direction: column;
      background: var(--vue-input-calculator-event-btn-bg-color);

      .header {
        color: var(--vue-input-calculator-text-color);
        align-self: flex-start;
        position: sticky;
        top: 0;
        left: 0;
        padding-top: 5px;
      }

      span {
        color: var(--vue-input-calculator-text-color);
        opacity: 0.75;
        display: block;
        font-size: 1rem;
        text-align: right;
        padding: 0.4rem 0;
        line-height: 1;
        font-weight: lighter;
      }
    }

    .calculator-input {
      color: var(--vue-input-calculator-text-color);
      width: 100%;
      border: none;
      padding: 0.8rem;
      display: block;
      font-size: 2.4rem;
      background: none;
      text-align: right;
      font-weight: lighter;

      &:focus,
      &:active {
        outline: none;
      }
    }

    .calculator-row {
      display: flex;
      padding: 0;
      justify-content: space-around;

      .calculator-col {
        flex: 1;
        box-shadow: 0 0 0 1px var(--vue-input-calculator-bg-color);

        &.wide {
          flex: 2;
        }
      }
    }

    .calculator-btn {
      width: 100%;
      color: var(--vue-input-calculator-text-color);
      border: none;
      cursor: pointer;
      padding: 0.3rem;
      outline: none;
      font-size: 2rem;
      transition: all 0.3s ease-in-out;
      font-weight: 200;
      justify-content: center;
      background-color: var(--vue-input-calculator-number-btn-bg-color);

      &.success {
        background-color: var(
          --vue-input-calculator-action-success-btn-bg-color
        );
        color: var(--vue-input-calculator-text-color);
      }

      &.accent {
        background-color: var(--vue-input-calculator-action-btn-bg-color);
        color: var(--vue-input-calculator-text-color);
      }

      &.gray {
        background-color: var(--vue-input-calculator-event-btn-bg-color);
      }

      &.action {
      }

      &:active {
        background-color: var(--vue-input-calculator-bg-color);
      }
    }
  }

  // подложка под калькулятором
  .backdrop {
    background-color: rgba(0, 0, 0, 0.5);
    width: 100%;
    height: 100%;
    z-index: 0;
  }
}

// анимация движения калькулятора
.slide-enter-active {
  animation: slide-in 0.5s;
}

.slide-leave-active {
  animation: slide-in 0.3s reverse;
}

@keyframes slide-in {
  from {
    transform: translateY(100%);
  }
  to {
    transform: translateY(0);
  }
}

// анимация прозрачности фона - бекдропа
.opacity-enter-active {
  animation: opacity-in 0.5s;
}

.opacity-leave-active {
  animation: opacity-in 0.3s reverse;
}

@keyframes opacity-in {
  100% {
    opacity: 1;
  }
  0% {
    opacity: 0;
  }
}
</style>
