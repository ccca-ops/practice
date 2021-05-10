<template>
<div> 
        <div class="biaoti"   >
            NEXT PLAYER IS {{msg}}
        </div>
  <my-board></my-board>
</div>
</template>

<script>
import MyBoard from './components/Board';
export default{
  name : 'biaoti',
  data(){
    return{
      msg : 'X'
    }
  },
  components: {
    MyBoard
  },
  methods: {
    updatevalue : function(){
      //this.msg=value;
     return null
    }
  }
}
</script>



<style>
</style>