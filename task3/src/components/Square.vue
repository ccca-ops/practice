<template>
 <div>
    <button class="square" v-if="val != '' " :disabled="dis">
      {{val}}
    </button>
    <button class="square" v-else @click="onclick(index)">
      {{val}}
    </button>
</div>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue'

export default defineComponent({
  name: 'square',
  props: {
    index: {
      type: Number,
      default: null
    },
    val: {
      type: String,
      default: ' '
    }
  },
  setup (props, context) {
    const dis = ref(true)

    const onclick = (index : number) => {
      context.emit('change', index)
    }
    return {
      onclick,
      dis
    }
  }
})
</script>

<style lang="scss">
.square {
  background: #fff;
  border: 1px solid #999;
  float: left;
  font-size: 24px;
  font-weight: bold;
  line-height: 34px;
  height: 34px;
  margin-right: -1px;
  margin-top: -1px;
  padding: 0;
  text-align: center;
  width: 34px;
}
button:disabled{
    color:black;
}
</style>
