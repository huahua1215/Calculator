<script setup>
import { ref, watch, computed } from "vue";

const calculation = ref("");
const tempResult = ref("");

// 輸入
const append = (operate) => {
  calculation.value += operate.toString();
};

// 清除（ＡＣ）
const clear = () => {
  calculation.value = "";
  tempResult.value = "";
};
// 計算結果
const result = computed(() => {
  if (!isNaN(calculation.value.slice(-1))) {
    return eval(calculation.value); //將字符串解析為JS表達式
  } else {
    return eval(calculation.value.slice(0, -1));
  }
});

// 監聽輸入的變化
watch(calculation, (newValue, oldValue) => {
  if (
    newValue !== "" && //不為空
    !isNaN(newValue.slice(-1)) && //最後一個字是數字
    newValue !== result.value && //不等於之前的計算結果
    newValue.length > 1
  ) {
    tempResult.value = result.value.toString();
  }
});
//
const filterOper = computed(() => (value) => {
  return value
    .replaceAll("*", " x ")
    .replaceAll("/", " ÷ ")
    .replaceAll("+", " + ")
    .replaceAll("-", " - ");
});
</script>

<template>
  <!-- 顯示區塊 -->
  <div class="screen-container">
    <div class="result">
      <transition name="slide-fade">
        <div class="number" v-if="tempResult !== ''" v-cloak>
          {{ tempResult }}
        </div>
      </transition>
    </div>

    <div class="calculation">
      <div class="number" v-cloak>{{ filterOper(calculation) }}</div>
    </div>
  </div>

  <!-- 按鈕區塊 -->
  <div class="btn-container flex">
    <div class="oper-container">
      <div class="operations" @click="clear()">
        <p>AC</p>
      </div>
      <div class="operations" @click="append('/')">
        <p>÷</p>
      </div>
      <div class="operations" @click="append('*')">
        <p>x</p>
      </div>
      <div class="operations" @click="append('+')">
        <p>+</p>
      </div>
      <div class="operations" @click="append('-')">
        <p>-</p>
      </div>
    </div>
    <div class="num-container grid grid-rows-4 grid-flow-col">
      <div class="btn" @click="append('7')">
        <p>7</p>
      </div>
      <div class="btn" @click="append('4')">
        <p>4</p>
      </div>
      <div class="btn" @click="append('1')">
        <p>1</p>
      </div>
      <div class="btn" @click="append('.')">
        <p>.</p>
      </div>
      <div class="btn" @click="append('8')">
        <p>8</p>
      </div>
      <div class="btn" @click="append('5')">
        <p>5</p>
      </div>
      <div class="btn" @click="append('2')">
        <p>2</p>
      </div>
      <div class="btn" @click="append('0')">
        <p>0</p>
      </div>
      <div class="btn" @click="append('9')">
        <p>9</p>
      </div>
      <div class="btn" @click="append('6')">
        <p>6</p>
      </div>
      <div class="btn" @click="append('3')">
        <p>3</p>
      </div>
      <div class="btn" @click="getResult()">
        <p>=</p>
      </div>
    </div>
  </div>
</template>

<style scoped>
.screen-container {
  width: 390px;
  height: 200px;
  position: relative;
  left: 75px;
  background-color: rgb(0, 0, 0);
  color: aliceblue;
  border-top-right-radius: 10px;
  border-top-left-radius: 10px;
}
.result,
.calculation {
  max-width: 340px;
  overflow-x: hidden;
  position: absolute;
  right: 20px;
}
.result {
  top: 15%;
  font-size: 50px;
  color: rgb(137, 141, 255);
  font-weight: bold;
}
.calculation {
  top: 50%;
  font-size: 25px;
}
.btn-container {
  font-size: 20px;
  cursor: pointer;
}
.oper-container {
  border: 1px solid black;
  border-radius: 15px;
  position: relative;
  top: -90px;
  left: 20px;
}
.oper-container .operations {
  padding: 20px;
  background-color: rgb(255, 255, 255);
  color: rgb(98, 41, 255);
}
.oper-container .operations:last-child {
  border-bottom-left-radius: 15px;
  border-bottom-right-radius: 15px;
}
.oper-container .operations:first-child {
  background-color: rgb(42, 42, 42);
  color: white;
  border-radius: 15px;
  border-bottom-left-radius: 0px;
  border-bottom-right-radius: 0px;
}

.num-container {
  background-color: rgb(42, 42, 42);
  color: white;
  border-bottom-left-radius: 15px;
  border-bottom-right-radius: 15px;
}
.num-container .btn {
  padding: 20px 60px;
}
</style>
