<template>
  <div class="area" @mouseleave="mouseleave">
    <h1>移动这个小方块！</h1>
    <div id="box" class="box" @touchstart="touchstart" @mousedown.left="mousedown" @touchend="touchend"
      @mouseup="mouseup"></div>
    <h1>（ {{ X }} , {{ Y }} ）</h1>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue';

let move: any = null
let WT_W: any = null
let WT_H: any = null
let CK_X: any = null
let CK_Y: any = null

let moveing = 0

let X: any = ref(0)
let Y: any = ref(0)
const mousedown = (box: any) => { //在鼠标点下的时候抓紧 移动
  // console.log(box.layerX)
  // console.log(box.layerY)
  // console.log(box.target)
  // console.log(box.target.clientWidth) //【物体宽度w】
  // console.log(box.target.clientHeight) //【物体高度h】
  WT_W = box.target.clientWidth
  WT_H = box.target.clientWidth
  move = (e: any) => {
    // console.log(e.clientX)
    // console.log(e.clientY)
    // console.log(e)
    // console.log(e.view.innerWidth) //【窗口x】
    // console.log(e.view.innerHeight)//【窗口y】
    CK_X = e.view.innerWidth
    CK_Y = e.view.innerHeight
    let left = e.clientX
    let top = e.clientY
    let boxSeatX = left - box.layerX
    let boxSeatY = top - box.layerY
    if (boxSeatX < 0) boxSeatX = 0 //当：box的x点小于窗口的 横方向x 
    if (boxSeatX > (CK_X - WT_W)) boxSeatX = CK_X - WT_W//当：box的x点大于 【窗口-物体宽度】的方向 
    if (boxSeatY < 0) boxSeatY = 0
    if (boxSeatY > (CK_Y - WT_H)) boxSeatY = CK_Y - WT_H//当：box的x点大于 【窗口-物体宽度】的方向 
    box.target.style.left = boxSeatX + "px";
    box.target.style.top = boxSeatY + "px";

    X.value = boxSeatX
    Y.value = boxSeatY
  }
  window.addEventListener('mousemove', move)
  moveing = 1
}

const mouseup = () => {  //松开鼠标的时候 取消监听
  console.log("取消监听")
  window.removeEventListener('mousemove', move)
  moveing = 0
}

const mouseleave = () => { //离开外框框的时候
  if (moveing == 1) {
    console.log("取消监听")
    window.removeEventListener('mousemove', move)
    moveing = 0
  }
}
window.onresize = function () {
  console.log("窗口变化了！")
  mouseleave();
  let box: any = document.getElementById("box");
  box.style.left = "calc(50% - 50px)";
  box.style.top = "calc(50% - 50px)";
}


// 以下是触摸拖拽
let touch: any = null
const touchstart = (box: any) => {
  // console.log(box)
  // WT_W = box.target.clientWidth
  // WT_H = box.target.clientWidth
  touch = (e: any) => {
    console.log(e.targetTouches[0].pageX)
    console.log(e.targetTouches[0].pageY)
    console.log(e)
    // console.log(e.view.innerWidth) //【窗口x】
    // console.log(e.view.innerHeight)//【窗口y】
    // CK_X = e.view.innerWidth
    // CK_Y = e.view.innerHeight
    let left = e.targetTouches[0].pageX
    let top = e.targetTouches[0].pageY
    let boxSeatX = left - box.layerX
    let boxSeatY = top - box.layerY
    // if (boxSeatX < 0) boxSeatX = 0 //当：box的x点小于窗口的 横方向x 
    // if (boxSeatX > (CK_X - WT_W)) boxSeatX = CK_X - WT_W//当：box的x点大于 【窗口-物体宽度】的方向 
    // if (boxSeatY < 0) boxSeatY = 0
    // if (boxSeatY > (CK_Y - WT_H)) boxSeatY = CK_Y - WT_H//当：box的x点大于 【窗口-物体宽度】的方向 
    box.target.style.left = boxSeatX + "px";
    box.target.style.top = boxSeatY + "px";

    X.value = boxSeatX
    Y.value = boxSeatY
  }
  window.addEventListener('touchmove', touch)
}

const touchend = () => {
  window.removeEventListener('touchmove', touch)
}
</script>

<style>
* {
  padding: 0;
  margin: 0;
  user-select: none;
}

.area {
  user-select: none;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  width: 100%;
  height: 100vh;
  background-color: #222;
  display: flex;
  flex-direction: column;
  justify-content: space-evenly;
  align-items: center;
  overflow: hidden;
}

.box {
  width: 100px;
  height: 100px;
  /* background-color: #f62072; */
  /* background-color: #1a1a1a; */
  position: absolute;
  border-radius: 10px;
  cursor: pointer;
}

.box:active {
  /* border: 1px solid #955365;
  box-shadow: 0px 0px 12px -2px #cf4b6f; */
  /* transform: scale(0.96);
  transition: transform 0.5s;
  -moz-transition: transform 0.5s;
  -webkit-transition: transform 0.5s;
  -o-transition:transform 0.5s; */
}

.box::after{
  content: '';
  display: block;
  width: 100px;
  height: 100px;
  border-radius: 12px;
  background-color: #cf4b6f;
  box-shadow: 0px 0px 20px -6px #cf4b6f;
  box-sizing: border-box;
}
.box:active::after{
  box-shadow: 0px 0px 20px -6px #cf4b6f;
  transform: scale(0.9);
  transition: transform 0.5s;
  -moz-transition: transform 0.5s;
  -webkit-transition: transform 0.5s;
  -o-transition:transform 0.5s;
}
</style>
