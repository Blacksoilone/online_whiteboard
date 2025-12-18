<script setup>
import { ref, onMounted } from 'vue'

let ctx = null
let lastX = 0
let lastY = 0
const whiteboard = ref(null)
let pathPoints = []

onMounted(() => {
  const canvas = whiteboard.value
  if (!canvas) return
  ctx = canvas.getContext('2d')
  if (!ctx) return

  ctx.lineWidth = 3
  ctx.lineCap = 'round'
  ctx.strokeStyle = '#000'
})

  let isDrawing = false
  function startDrawing(e) { 
    console.log("你竟然敢按下鼠标？！")
    isDrawing = true
    const rect = whiteboard.value.getBoundingClientRect()
    lastX = Math.round(e.clientX - rect.left)
    lastY = Math.round(e.clientY - rect.top)

    pathPoints=[]
    pathPoints.push([lastX, lastY])
  }
  function draw(e) { 
    if (!isDrawing||!ctx) return
    console.log("你竟然还敢移动鼠标？！")
    const rect = whiteboard.value.getBoundingClientRect()
    const x = Math.round(e.clientX - rect.left)
    const y = Math.round(e.clientY - rect.top)

    ctx.beginPath()
    ctx.moveTo(lastX, lastY)
    ctx.lineTo(x, y)
    ctx.stroke()

    pathPoints.push([x, y])

    lastX = x
    lastY = y
  }
  function stopDrawing(e) { 
    isDrawing = false
    console.log("你甚至还敢抬起鼠标？！")

    console.log("你太邪恶了，你竟然留下了这么多数据", pathPoints)

    console.log("为了惩戒你，我要把你的数据发给可怕的王松川", JSON.stringify(pathPoints))

  }
  function stopDrawing_leave(e) { 
    isDrawing = false
    console.log("你居然还敢离开画板？！")
  }
  function clearCanvas() { 
    console.log("你怎么敢清空画布的？！")
    ctx.clearRect(0, 0, whiteboard.value.width, whiteboard.value.height)
  }

</script>

<template>
  <p class = "whiteboard-title">
    Whiteboard
  </p >
  <button class="clear-button" @click="clearCanvas">摧毁这一切</button>
  <div class="whiteboard-app">

    <canvas ref="whiteboard"
      width="800"
      height="600"
      @mousedown="startDrawing"
      @mousemove="draw"
      @mouseup="stopDrawing"
      @mouseleave="stopDrawing_leave"
    ></canvas>
  </div>
</template>

<style>
  .whiteboard-title {
    text-align: center;
    font-size: 20px;
    font-weight: bold;
    margin-bottom: 20px;
  }
  .clear-button {
    display: block;
    margin: 0 auto;
    padding: 10px 20px;
    background-color: rgb(73, 47, 93);
    color: rgb(201, 255, 185);
    border: none;
    border-radius: 5px;
    cursor: pointer;
  }
  .whiteboard-app {
    display: flex;
    justify-content: center;
    align-items: center;
    margin-top: 20px;
  }
  canvas {
    border: 1px solid black;
  }

</style>
