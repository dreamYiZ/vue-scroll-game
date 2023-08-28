<script setup>
defineProps(['whatUGot', 'showWhatUGot'])
defineEmits(['ok-click'])
</script>


<template>
  <div class="what-u-got-overlay" v-show="showWhatUGot">
    <div class="what-u-got-box">
        
      <div>
        <div class="happy-got-text">
            恭喜获得！
        </div>
        <img
          class="scroll-item"
          :src="item.img"
          alt=""
          v-for="(item, idx) in whatUGot"
          :key="idx"
        />
      </div>
      <button @click="$emit('ok-click')">确定</button>
    </div>
  </div>
</template>

<style lang="less" scoped>
.what-u-got-overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgba(0, 0, 0, 0.4);
}
.scroll-item {
  width: 100px;
  height: 100px;
  margin-right: 20px;
}
.what-u-got-box{
    width: 600px;
    height: 400px;
    margin: 20vh auto 0;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    align-items: center;
    background-color: #000000;
    padding: 20px;
}
.happy-got-text{
    font-size: 24px;
    color: red;
    text-align: center;
}
</style>