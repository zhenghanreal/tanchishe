<template>
    <div class="gamemain">
        <ul>
            <li v-for="(item, index) in gameMap" :key="index" :class="{
                header: item.header,
                body: item.body,
                food: item.food
            }">
            </li>
        </ul>
    </div>
    <div class="score">得分: {{score}}</div>
    <div class="control">
        <button class="start" @click="startGame">开始</button>
        <button class="stop" @click="stopGame">暂停</button>
        <button class="restart" @click="restartGame">重置</button>
    </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
// 生成地图
const gameMap = ref([])
const createMap = ()=>{
    for(let i = 0; i < 600; i++){
        gameMap.value.push({})
    }
}
// 生成蛇
const snake = ref([45,46,47])
const createSnake = () => {
    gameMap.value.forEach(item => {
        item.header = false
        item.body = false
    })
    gameMap.value[snake.value[snake.value.length-1]].header = true
    for(let i = 0; i < snake.value.length-1; i++){
        gameMap.value[snake.value[i]].body = true
    }
}
// 前进方向
const direction = ref('right')
// 移动
const move = () => {
  const headerIndex = snake.value[snake.value.length-1]
  // 边界判定条件
  const borderArr = [
    direction.value === 'right' && (headerIndex+1)%40 === 0,
    direction.value === 'left' && (headerIndex)%40 === 0,
    direction.value === 'up' && (headerIndex)<40,
    direction.value === 'down' && (headerIndex+1)>560
  ]
  if( borderArr.includes(true) ){
    clearInterval(timer.value)
    alert('游戏结束，你获得了'+score.value+'分')
    restartGame()
    return
  }
  // 头撞到身体
  if(gameMap.value[headerIndex].body === true){
    clearInterval(timer.value)
    alert('游戏结束，你获得了'+score.value+'分')
    restartGame()
    return
  }
  // 清除样式
  gameMap.value.forEach(item => {
      item.header = false
      item.body = false
  })
  // 判断下一步的方向
  if(direction.value === 'right'){
    snake.value.splice(0,1)
    snake.value.push(headerIndex+1)
    createSnake()
  }else if(direction.value === 'left'){
    snake.value.splice(0,1)
    snake.value.push(headerIndex-1)
    createSnake()
  }else if(direction.value === 'up'){
    snake.value.splice(0,1)
    snake.value.push(headerIndex-40)
    createSnake()
  }else if(direction.value === 'down'){
    snake.value.splice(0,1)
    snake.value.push(headerIndex+40)
    createSnake()
  }
}
// 生成食物
const food = ref(null)
const createFood = ()=>{
    gameMap.value.forEach(item=>{
        item.food = false
    })
    gameMap.value.forEach(item => {
        if(item.header === true){
            food.value = Math.floor(Math.random()*600)
            item.body = false
            gameMap.value[food.value].food = true
        }
    })
}
// 吃食物
const eatFood = ()=>{
    if(snake.value[snake.value.length-1] === food.value){
        snake.value.unshift(snake.value[0])
        score.value+=10
        createFood()
    }
}
// 得分
const score = ref(0)
// 开始游戏
const startGame = ()=>{
    if(timer.value){
        clearInterval(timer.value)
        timer.value = null
    }else{
        timer.value = setInterval(()=>{
            move()
            eatFood()
        },100)
    }
}
// 暂停游戏
const stopGame = () => {
    clearInterval(timer.value)
}
// 重新开始
const  restartGame = ()=>{
    score.value = 0
    clearInterval(timer.value)
    timer.value = null
    snake.value = [45,46,47]
    direction.value = 'right'
    createFood()
    createSnake()
}
// 计时器
const timer = ref(null)
onMounted(()=>{
    createMap()
    createSnake()
    createFood()
    // 键盘弹起监听
    document.onkeydown = (e)=>{
        switch(e.keyCode){
            case 37:
                if(direction.value !== 'right'){
                    direction.value = 'left'
                }
                break
            case 38:
                if(direction.value !== 'down'){
                    direction.value = 'up'
                }
                break
            case 39:
                if(direction.value !== 'left'){
                    direction.value = 'right'
                }
                break
            case 40:
                if(direction.value !== 'up'){
                    direction.value = 'down'
                }
                break
        }
   }
})
</script>

<style lang="scss" scoped>
.gamemain {
    margin: 0 auto;
    width: 1200px;
    height: 450px;
    border: 1px solid rgba(255, 255, 255, 0.5);
    ul{
        display: flex;
        flex-wrap: wrap;
        li{
            box-sizing: border-box;
            width: 30px;
            height: 30px;
            border: 1px solid rgba(255, 255, 255, 0.5);
        }
    }
}
.score {
    position: fixed;
    top: 50px;
    right: 50px;
    background-color:#ffdd33;
    height: 50px;
    width: 150px;
    text-align: center;
    line-height: 50px;
    font-size: 20px;
    color: #cc5200;
    font-weight: bolder;
    border: 3px solid #ff6600;
    border-radius: 50px;
}
.control {
    margin: 0 auto;
    margin-top: 20px;
    width: 1200px;
    height: 100px;
    display: flex;
    justify-content: space-around;
    align-items: center;
    button {
      background-color: #ffcc00;
      border: 3px solid #ff6600;
      border-radius: 15px;
      color: #333;
      font-size: 20px;
      font-weight: bold;
      padding: 10px 20px;
      text-align: center;
      text-decoration: none;
      display: inline-block;
      cursor: pointer;
      box-shadow: 0 5px #cc5200;
      transition: all 0.2s ease;
    }

    button:active {
      box-shadow: 0 2px #cc5200;
      transform: translateY(3px);
    }

    button:hover {
      background-color: #ffdd33;
    }
}
.header::before{
    content: '';
    display: block;
    background-color: rgb(255, 255, 1);
    width: 30px;
    height: 30px;
    border-radius: 50%
}
.body::before{
    content: '';
    display: block;
    background-color: rgb(208, 208, 0);
    width: 30px;
    height: 30px;
    border-radius: 50%
}
.food::before{
    content: '';
    display: block;
    background-color: rgb(252, 168, 2);
    width: 30px;
    height: 30px;
    border-radius: 50%
}
</style>