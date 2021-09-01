<template>
  <div id="app">
    <section>
      <div class="container">
        <div id="data-wrapper">
          <div>
            <p class="answer" v-html="this.total !== '' ? 'Ans = ' + this.total : ''">Ans = {{ this.total }}</p>
            <input v-model="displayData" ref="email" type="text" />
          </div>
        </div>

        <div id="buttons-wrapper">
          <div>
            <div class="dark-grey two-btn">
              <button class="dark-grey">Rad</button>
              <button class="dark-grey disable">Deg</button>
            </div>
            <button class="dark-grey">x!</button>
            <button class="dark-grey">(</button>
            <button class="dark-grey">)</button>
            <button class="dark-grey">%</button>
            <button class="dark-grey" @click="clearFn()">CE</button>
          </div>

          <div>
            <button class="dark-grey">Inv</button>
            <button class="dark-grey">sin</button>
            <button class="dark-grey">In</button>
            <button class="light-grey num" @click="appendFn('7')">7</button>
            <button class="light-grey num" @click="appendFn('8')">8</button>
            <button class="light-grey num" @click="appendFn('9')">9</button>
            <button class="dark-grey operator" @click="divideFn('÷')">÷</button>
          </div>

          <div>
            <button class="dark-grey">π</button>
            <button class="dark-grey">cos</button>
            <button class="dark-grey">log</button>
            <button class="light-grey num" @click="appendFn('4')">4</button>
            <button class="light-grey num" @click="appendFn('5')">5</button>
            <button class="light-grey num" @click="appendFn('6')">6</button>
            <button class="dark-grey operator" @click="mulFn('×')">×</button>
          </div>

          <div>
            <button class="dark-grey">e</button>
            <button class="dark-grey">tan</button>
            <button class="dark-grey">√</button>
            <button class="light-grey num" @click="appendFn('1')">1</button>
            <button class="light-grey num" @click="appendFn('2')">2</button>
            <button class="light-grey num" @click="appendFn('3')">3</button>
            <button class="dark-grey operator" @click="subFn('-')">−</button>
          </div>

          <div>
            <button class="dark-grey">Ans</button>
            <button class="dark-grey">EXP</button>
            <button class="dark-grey">x <sup>y</sup></button>
            <button class="light-grey num"  @click="appendFn('0')">0</button>
            <button class="light-grey operator"  @click="dotFn('.')">.</button>
            <button class="equals" @click="equalFn()">=</button>
            <button class="dark-grey operator" @click="addFn('+')">+</button>
          </div>
        </div>
      </div>
    </section>
  </div>
</template>

<script>

export default {
  name: 'App',
  data() {
    return {
      previous: null,
      current: '',
      getData: [],
      total: '',
      operator: null,
      operatorClicked: false
    }
  },
  computed: {
    displayData(){
      return this.getData.length === 0 ? '0' : this.getData.join('')
    }
  },
  methods: {
    setPrevious(){
      this.previous = this.current
      this.operatorClicked = true
    },
    addFn(data) {
      this.operator = (a,b) => a + b
      this.setPrevious()
      this.getData.push(data)
    },
    subFn(data) {
      this.operator = (a,b) => b - a
      this.setPrevious()
      this.getData.push(data)
    },
    mulFn(data) {
      this.operator = (a,b) => a * b
      this.setPrevious()
      this.getData.push(data)
    },
    divideFn(data) {
      this.operator = (a,b) => a / b
      this.setPrevious()
      this.getData.push(data)
    },
    equalFn() {
      this.current = this.operator(
        parseFloat(this.current),
        parseFloat(this.previous)
      )
      this.total = this.current
      this.previous = null
    },
    clearFn() {
      this.current = '',
      this.total = '',
      this.getData = []
    },
    appendFn(data){
      if(this.operatorClicked){
        this.current = ''
        this.operatorClicked = false
      }
      this.current = `${this.current}${data}`
      this.getData.push(data)
    },
    dotFn() {
      if(this.current.indexOf('.') === -1){
        this.appendFn('.')
      }
    }
  }
}
</script>

<style lang="scss">
html * {
  font-family: 'Arial', sans-serif;
}
.container {
  max-width: 800px;
  margin: auto;
}

#data-wrapper {
  border: 1px solid #ebebeb;
  border-radius: 8px;
  height: 72px;
  padding: 10px 14px 5px 10px;
  box-sizing: border-box;
  display: flex;
  justify-content: flex-end;
  text-align: right;

  > div {
    width: 100%;
  }

  .answer{
    margin: 0;
    color: #70757a;
    font-size: 13px;
    min-height: 25px;
  }

  input {
    -webkit-appearance: none;
    outline: none;
    border: none;
    width: 100%;
    font-size: 30px;
    padding: 0;
    text-align: right;
  }
}

#buttons-wrapper{
  > div {
    display: flex;
    grid-gap: 8px;
    margin-top: 8px;
  }

  .two-btn {
    min-width: 221px;
    width: 100%;
    display: flex;
    position: relative;

    &:before{
      content: "";
      position: absolute;
      top: 10px;
      bottom: 10px;
      left: 50%;
      border-left: 1px solid #70757a;
      transform: translateX(-50%);
    }
  }

  .dark-grey {
    background: #dadce0;
    color: #202124;
    border: 1px solid #dadce0;
    border-radius: 4px;

    &:active {
      border-color: #70757a;
    }
  }

  .light-grey {
    background: #f1f3f4;
    color: #202124;
    border: 1px solid #f1f3f4;
    border-radius: 4px;

    &:active {
      border-color: #dadce0;
    }
  }

  .equals{
    background: #4285f4;
    color: #fff;
    border: 1px solid #4285f4;
    border-radius: 4px;

    &:active {
      border-color: #1558d6;
    }
  }

  .disable {
    color: #70757a;
  }
  .num {
    font-size: 14px;
    font-weight: 400;
  }
  .operator {
    font-size: 18px;
    font-weight: 400;
  }
  button {
    min-width: 25px;
    max-width: 110px;
    width: 100%;
    height: 36px;
    cursor: pointer;

    &:active {
      box-shadow: 0 1px 6px rgba(32, 33, 36, .28);
    }
  }
}
</style>
