<template>
    <div class="calc">
      <div class="display">{{numericSpy || '0'}}</div>
      <div class="btn" @click="clear">C</div>
      <div class="btn" @click="sign">+/-</div>
      <div class="btn" @click="percent">%</div>
      <div class="btn operator" @click="divide">÷</div>
      <div class="btn" @click="append(7)">7</div>
      <div class="btn" @click="append(8)">8</div>
      <div class="btn" @click="append(9)">9</div>
      <div class="btn operator" @click="times">x</div>
      <div class="btn" @click="append(4)">4</div>
      <div class="btn" @click="append(5)">5</div>
      <div class="btn" @click="append(6)">6</div>
      <div class="btn operator" @click="minus">-</div>
      <div class="btn" @click="append(1)">1</div>
      <div class="btn" @click="append(2)">2</div>
      <div class="btn" @click="append(3)">3</div>
      <div class="btn operator" @click="add">+</div>
      <div class="btn zero" @click="append(0)">0</div>
      <div class="btn" @click="dot">.</div>
      <div class="btn operator" @click="equal">=</div>
    </div>
</template>

<script>

export default {
    name: "Calculator",

    data() {
      return {
        previous: null,
        current: "",
        operator: null,
        operatorClicked: false,
      }
    },

    methods: {
      clear() {
        this.current = "";
      },

      sign() {
        this.current = this.current.charAt(0) === "-" ?
            this.current.slice(1) : `-${this.current}`;
      },

      percent() {
        this.current = `${parseFloat(this.current) / 100}`;
      },

      append(num) {
        if (this.operatorClicked) {
          this.current = "";
          this.operatorClicked = false;
        }
        this.current += num;
      },

      dot() {
        if (this.current.indexOf(".") === -1) {
          this.append(".");
        }
      },

      setPrevious() {
        this.previous = this.current;
        this.operatorClicked = true;
      },

      divide() {
        this.operator = (a, b) => a / b;
        this.setPrevious();
      },

      times() {
        this.operator = (a, b) => a * b;
        this.setPrevious();
      },

      minus() {
        this.operator = (a, b) => a - b;
        this.setPrevious();
      },

      add() {
        this.operator = (a, b) => a + b;
        this.setPrevious();
      },

      equal() {
        this.current = `${this.operator(
            parseFloat(this.previous),
            parseFloat(this.current)
        )}`;
        this.previous = null;
      }
    },

    computed: {
      numericSpy: function() {
        if (this.current.length >= 17) {
          alert("Вы превысили лимит на количество чисел, последующие числа не будут видны!");
        }

        return this.current;
      }
    }
}

</script>

<style lang="scss" scoped>
.calc {
  width: 400px;
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-auto-rows: minmax(50px, auto);
  margin: 0 auto;
  font-size: 40px;
  box-shadow: 0 0 10px #35495e;
}

.display {
  grid-column: 1/5;
  background-color: #35495e;
  color: white;
  overflow: hidden;
}

.zero {
  grid-column: 1/3;
}

.btn {
  background-color: #41b883;
  border: 1px solid #333;
  border-collapse: collapse;
  cursor:pointer
}

.btn:active {
  transform: scale(0.95);
}

.operator {
  background-color: orange;
  color: white;
}
        
</style>