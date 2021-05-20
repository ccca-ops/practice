<template>
    <div class="boardrow">
        <div v-if=" win != '' "><h3>winner is {{win}}</h3></div>
        <div v-else ><h3>NEXT PLAYER IS {{nowvalue}}</h3></div>
        <div class="boardrow" v-for="(num,index) in list" :key="num" >
            <mysquare  :index=index :val=list[index].val
                        @change="changeval(index)">
            </mysquare>
            <div v-if="(index+1)%3==0" class="extra"></div>
    </div>
        <li v-for="(item,index) in stepnum" :id="'myid'+index" :key="item">
            <button @click="jumpto(index)" >{{ stepnum[index] }}</button>
        </li>
    </div>
</template>

<script lang="ts">
import { defineComponent, reactive, ref } from 'vue'
import mysquare from './Square.vue'

export default defineComponent({
  name: 'boardrow',
  components: {
    mysquare
  },
  setup () {
    const list = ref(
      [{ num: 0, val: '' },
        { num: 1, val: '' },
        { num: 2, val: '' },
        { num: 3, val: '' },
        { num: 4, val: '' },
        { num: 5, val: '' },
        { num: 6, val: '' },
        { num: 7, val: '' },
        { num: 8, val: '' }])
    const xISnext = ref(true)
    const nowvalue = ref('X')
    const judge = ref('')
    const desc = ref(0) // 当前步数
    const stepnum = ref(Array(1).fill('Go to game star')) // 生成按钮的数组
    const history = ref([['', '', '', '', '', '', '', '', '']])

    const changeval = (index : number) => {
      xISnext.value = !xISnext.value

      // 禁用点击棋盘的：
      judge.value = calculateWinner() as string
      if (judge.value === 'X' || judge.value === 'O') {
        return null
      }

      if (xISnext.value === false) {
        list.value[index].val = 'X'
        nowvalue.value = 'O' // 显示next玩家
      } else {
        list.value[index].val = 'O'
        nowvalue.value = 'X'
      }

      desc.value = desc.value + 1 // 当前步数
      stepnum.value[stepnum.value.length] = 'Go to move#' + desc.value // 把当前步数值desc存入stepnum
      stepnum.value = stepnum.value.slice(0, desc.value + 1) // 根据步骤数更新按钮数组

      // 历史数组
      history.value[desc.value + 1] = reactive(Array(1))
      for (let j = 0; j < 9; j++) {
        history.value[desc.value + 1][j] = list.value[j].val
      }
    }

    const win = ref('')
    const jumpto = (index : number) => { // 这里的index相当于desc，是history的步骤
      for (let i = 0; i < 9; i++) {
        if (index === 0)list.value[i].val = '' // 点击gamestar情况
        else list.value[i].val = history.value[index + 1][i]
      }
      desc.value = index // 改变当前步骤数
      win.value = ''
      if (desc.value % 2 === 1) {
        xISnext.value = false
        nowvalue.value = 'O'
      }
      if (desc.value % 2 === 0) {
        xISnext.value = true
        nowvalue.value = 'X'
      }
    }

    const calculateWinner = () => {
      const lines = [
        [0, 1, 2],
        [3, 4, 5],
        [6, 7, 8],
        [0, 3, 6],
        [1, 4, 7],
        [2, 5, 8],
        [0, 4, 8],
        [2, 4, 6]
      ]
      for (let i = 0; i < lines.length; i++) {
        const [a, b, c] = lines[i]
        if (list.value[a].val && list.value[a].val === list.value[b].val && list.value[a].val === list.value[c].val) {
          win.value = list.value[a].val
          return list.value[a].val
        }
      }
    }

    return {
      win,
      nowvalue,
      list,
      stepnum,
      changeval,
      jumpto,
      calculateWinner
    }
  }
})
</script>

<style lang="scss">
.extra{
    height: 33px;
}
</style>
