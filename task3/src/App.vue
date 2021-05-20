<template>
  <div class="blank">
    <my-board/>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'
import MyBoard from './components/Board.vue'

export default defineComponent({
  name: 'blank',
  components: {
    MyBoard
  }
})
</script>

<style lang="scss">

</style>
