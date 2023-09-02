<script setup>
// import { RouterLink, RouterView } from 'vue-router'
// import HelloWorld from './components/HelloWorld.vue'
import WhatUGotView from './components/WhatUGotView.vue';

import { onMounted, ref } from 'vue'

const base_fruits = [
  {
    img: '/apple1.jpeg',
    name: 'apple1'
  },
  {
    img: '/pear.jpg',
    name: 'pear'
  },
  {
    img: '/banana.jpg',
    name: 'banana'
  },
  {
    img: '/apple2.jpeg',
    name: 'apple2'
  }
]

function randomIntFromInterval(min, max) {
  // min and max included
  return Math.floor(Math.random() * (max - min + 1) + min)
}

const pickRandomFruit = () => {
  return base_fruits[randomIntFromInterval(0, base_fruits.length - 1)]
}

const generateFruits = (n) => {
  const fruits = []
  while (n) {
    fruits.push(pickRandomFruit())
    n--
  }
  return fruits
}

const dataScroll_1 = ref([])

const dataScroll_2 = ref([])

const dataScroll_3 = ref([])

const transX_1 = ref(0)
const transX_2 = ref(0)
const transX_3 = ref(0)

const showWhatUGot = ref(false)

const whatUGot = ref([])

const STEP_WIDTH = 100

// const scrollToIdx = ({
//   dataArr, targetIdx, time, transX
// })=>{
//   const maxWidth = STEP_WIDTH* dataArr.length;
//   const targetX = targetIdx * STEP_WIDTH;

// }

const scrollToIdx = ({ dataArr, targetIdx, time, transX }) => {
  const maxWidth = STEP_WIDTH * dataArr.value.length
  console.log('maxWidth', maxWidth)
  const targetX = targetIdx * STEP_WIDTH
  // use a quadratic easing function to calculate the transX value // https://easings.net/#easeOutQuad
  const easeOutQuad = (t, b, c, d) => {
    t /= d
    return -c * t * (t - 2) + b
  }
  // use requestAnimationFrame to animate the transX value
  // https://developer.mozilla.org/en-US/docs/Web/API/window/requestAnimationFrame
  let startTime = null
  const animate = (timestamp) => {
    console.log('animate')
    if (!startTime) startTime = timestamp
    let elapsed = timestamp - startTime
    if (elapsed > time * 1000) elapsed = time * 1000
    // limit the elapsed time to the given time
    transX.value = easeOutQuad(elapsed, 0, targetX, time * 1000)

    console.log('transX', transX.value, maxWidth)

    // update the transX value
    if (transX.value < targetX) {
      requestAnimationFrame(animate)
      // continue the animation
    }
  }
  requestAnimationFrame(animate) // start the animation
}
onMounted(() => {
  // 最开始生成水果
  dataScroll_1.value = generateFruits(299)
  dataScroll_2.value = generateFruits(299)
  dataScroll_3.value = generateFruits(299)


  console.log('dataScroll_1.value', dataScroll_1.value)
  console.log('dataScroll_2.value', dataScroll_2.value)
  console.log('dataScroll_3.value', dataScroll_3.value)
})
const onClickStart = () => {
  console.log('onClickStart')
  let time = 1

  // 滚动到目标位置
  scrollToIdx({
    dataArr: dataScroll_1,

    time,
    transX: transX_1,
    targetIdx: 152, // 第多少个
  })

  // 滚动到目标位置 2

  scrollToIdx({
    dataArr: dataScroll_2,

    time,
    transX: transX_2,
    targetIdx: 152, // 第多少个
  })

  // 滚动到目标位置 3行

  scrollToIdx({
    dataArr: dataScroll_3,

    time,
    transX: transX_3,
    targetIdx: 152,  // 第多少个
  })

  // z最后显示的水果
  whatUGot.value = [dataScroll_1.value[155], dataScroll_2.value[155], dataScroll_3.value[155]]
  setTimeout(() => {
    showWhatUGot.value = true
  }, time * 1000 + 1000 * 1)
}


</script>

<template>
  <div class="container-main">
    <div class="game-box">
      <div class="center-box"></div>
      <div class="scroll-container-c scroll-container-1">
        <div class="fruits-horizontal" :style="{ transform: `translateX(-${transX_1}px)` }">
          <img class="scroll-item" :src="item.img" alt="" v-for="(item, idx) in dataScroll_1" :key="idx" />
        </div>
      </div>
      <div class="scroll-container-c scroll-container-2">
        <div class="fruits-horizontal" :style="{ transform: `translateX(-${transX_2}px)` }">
          <img class="scroll-item" :src="item.img" alt="" v-for="(item, idx) in dataScroll_2" :key="idx" />
        </div>
      </div>
      <div class="scroll-container-c scroll-container-3">
        <div class="fruits-horizontal" :style="{ transform: `translateX(-${transX_3}px)` }">
          <img class="scroll-item" :src="item.img" alt="" v-for="(item, idx) in dataScroll_3" :key="idx" />
        </div>
      </div>
    </div>
    <button class="start-btn" @click="onClickStart()">开始抽奖</button>
  </div>
  <!-- <header>
    <img alt="Vue logo" class="logo" src="@/assets/logo.svg" width="125" height="125" />

    <div class="wrapper">
      <HelloWorld msg="You did it!" />

      <nav>
        <RouterLink to="/">Home</RouterLink>
        <RouterLink to="/about">About</RouterLink>
      </nav>
    </div>
  </header>

  <RouterView /> -->

  <WhatUGotView :whatUGot="whatUGot" :showWhatUGot="showWhatUGot" @ok-click="showWhatUGot = false" />
</template>

<style scoped lang="less">
.container-main {
  text-align: center;
}

.game-box {
  // border: 1px solid rgba(0, 0, 0, 0.5);
  outline: 1px solid rgba(0, 0, 0, 0.5);
  width: 700px;
  margin: 0 auto;
  position: relative;
}

.scroll-item {
  width: 100px;
  height: 100px;
}

.scroll-container-c {
  width: 100%;
  height: 100px;
  overflow: hidden;
  margin: 0 auto;
}

.start-btn {
  margin-top: 30px;
  margin-left: auto;
  margin-right: auto;
}

.fruits-horizontal {
  height: 100px;
  width: max-content;
}

.center-box {
  position: absolute;
  width: 100px;
  height: 100%;
  left: 50%;
  transform: translateX(-50%);
  border: 1px solid rgba(255, 0, 0, 0.5);
  z-index: 99;
}
</style>
