<script setup>
import { computed, onMounted, ref, watch } from 'vue'

let list = ref([])
let virtualList = ref([])
let scrollTop = ref(0)
let container = ref()
let number = 1000
let itemHeight = 100
let totalHeight = number * itemHeight

let temp = []
for (let i = 1; i < number; i++) {
  temp.push(i)
}

onMounted(() => {
  addData()
})

function addData() {
  list.value = temp
}

function onscroll() {
  scrollTop.value = container.value.scrollTop
  let startIndex = Math.floor(container.value.scrollTop / 100)
  let visibleCount = Math.ceil(container.value.clientHeight / 100)
  let end = startIndex + visibleCount
  virtualList.value = list.value.slice(startIndex, end)
}

const getTransform = computed(() => {
  return `translateY(${scrollTop.value}px)`
})
</script>

<template>
  <!--  <button @click="addData">add data</button>-->
  <div class="container" ref="container" @scroll="onscroll($event)">
    <div class="view-content" :style="{ height: totalHeight + 'px' }"></div>
    <div class="virtualList" :style="{ transform: getTransform }">
      <div
        class="item"
        :style="{ height: itemHeight + 'px' }"
        v-for="item in virtualList"
        :key="item"
      >
        item {{ item }}
      </div>
    </div>
  </div>
</template>

<style scoped>
.container {
  height: 100%;
  overflow: auto;
  position: relative;

  .view-content {
    position: absolute;
    top: 0px;
    left: 0px;
    right: 0px;
    z-index: -1;
  }

  .virtualList {
    position: absolute;
    top: 0px;
    left: 0px;
    right: 0px;
  }

  .item {
    border: 1px solid black;

    width: 100%;
    line-height: 100px;
    text-align: center;
  }
}
</style>
