<template>
  <div>
    <div class="title">
      <div class="text">贪吃蛇</div>
    </div>
  </div>
</template>

<script>
export default {

}
</script>

<style lang="scss" scoped>
.title {
  margin: 0 auto;
  position: relative;
  width: 700px;
  height: 150px;
  .text{
    font-weight: bolder;
    font-size: 70px;
    text-shadow: 1px -1px #fff, -1px 1px #999, -5px 5px 5px #808080;
    color:white;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%,-50%);
  }
}
</style>