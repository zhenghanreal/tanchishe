<template>
  <div class="home">
    <Title/>
    <GameMain/>
  </div>
</template>

<script>
// @ is an alias to /src
import Title from '@/components/Title.vue'
import GameMain from '@/components/GameMain.vue'

export default {
  name: 'HomeView',
  components: {
    Title,
    GameMain
  }
}
</script>
