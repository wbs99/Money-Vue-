<template>
  <div class="numberPad">
    <div class="output">{{ output }}</div>
    <div class="buttons">
      <button @click="inputContent">1</button>
      <button @click="inputContent">2</button>
      <button @click="inputContent">3</button>
      <button @click="remove">删除</button>
      <button @click="inputContent">4</button>
      <button @click="inputContent">5</button>
      <button @click="inputContent">6</button>
      <button @click="clear">清空</button>
      <button @click="inputContent">7</button>
      <button @click="inputContent">8</button>
      <button @click="inputContent">9</button>
      <button @click="inputContent">.</button>
      <button @click="inputContent" class="zero">0</button>
      <button @click="ok" class="ok">OK</button>
    </div>
  </div>
</template>
<script lang="ts">
import Vue from 'vue';
import {Component, Prop} from 'vue-property-decorator';

@Component
export default class NumberPad extends Vue {
  @Prop(Number) readonly value!: number;
  output = this.value.toString();

  inputContent(event: MouseEvent) {
    const button = (event.target as HTMLButtonElement);
    const input = button.textContent!;
    if (this.output.length === 16) { return; }
    if (this.output === '0') {
      if ('0123456789'.indexOf(input) >= 0) {
        this.output = input;
      } else {
        this.output += input;
      }
      return;
    }
    if (this.output.indexOf('.') >= 0 && input === '.') {return;}
    this.output += input;
  }

  remove() {
    if (this.output.length === 1) {
      this.output = '0';
    } else {
      this.output = this.output.slice(0, -1);
    }
  }

  clear() {
    this.output = '0';
  }

  ok() {
    const number = parseFloat(this.output);
    this.$emit('update:value', number);
    this.$emit('submit', number);
    this.output = '0';
  }
}
</script>
<style scoped lang="scss">
.numberPad {
  background: #a4c8c0;
  display: flex;
  flex-direction: column;

  .output {
    font-size: 26px;
    font-family: Consolas, monospace;
    text-align: right;
    border: 1px solid #333;
    border-radius: 10px;
    background: white;
    padding: 0 5px;
    margin: 4px 2px;
  }

  .buttons {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-around;
    flex-grow: 1;
    padding: 4px;
    margin: 1px 3px;

    > button {
      font-family: Helvetica Neue;
      font-weight:bold;
      width: 20%;
      border: 2px solid #333;
      border-radius: 10px;
      background: white;
      flex-grow: 1;
      padding: 8px;
      margin: 6px;
      font-size: 16px;
      flex-shrink: 1;
    }
  }
}
</style>