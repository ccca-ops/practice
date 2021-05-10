<template>
    <div class="boardrow"> 
            <div class="boardrow" v-for="(num,index) in list" :key="num" >
                <my-square  :index=index :val=list[index].val 
                        @change="changeval(index)"> 
                </my-square>  
                <div v-if="(index+1)%3==0" class="extra"></div>
            </div>
           <li v-for="(item,index) in stepnum" :id="'myid'+index" :key="item">
                <button @click="jumpto(index)" >{{ stepnum[index] }}</button>
           </li>
        <my-game ref="game"></my-game>
    </div>
</template>

<script>
import MyGame from './Game.vue';
import MySquare from './Square.vue';
export default {
        name : 'boardrow',
        data(){
            return{
                list : [
                    {num:0,val:''},
                    {num:1,val:''},
                    {num:2,val:''},
                    {num:3,val:''},
                    {num:4,val:''},
                    {num:5,val:''},
                    {num:6,val:''},
                    {num:7,val:''},
                    {num:8,val:''}],
                xISnext : true,
                nowvalue : 'X',
                judge : '',
                win :'',  
                desc :null,   //当前步数
                stepnum:Array(1).fill('Go to game star'),  //生成按钮的数组
                history:[ ['','','','','','','','',''] ],
            }  
        },
        components: {
            MySquare, MyGame,
        },
        methods: {
            changeval:function(index){
                    this.xISnext=!this.xISnext;
                    this.judge = this.$refs.game.calculateWinner(this.list);

                    history[history.length]=new Array();
                    for(let j = 0;j < 9;j++ ){
                        history[history.length][j]=this.list[j].val;
                    }console.log(history[history.length])

                    this.win=this.$refs.game.win;
                    //禁用点击棋盘的：
                    while(this.judge === 'X' || this.judge === 'O'){
                        return null
                    }
                    if(this.xISnext == false){
                        this.list[index].val='X';
                        this.nowvalue = this.list[index].val;
                    }else{
                        this.list[index].val='O';
                        this.nowvalue = this.list[index].val; 
                    }
                    this.desc=this.desc+1; //当前步数
                    this.stepnum[this.stepnum.length]='Go to move#'+this.desc;   //把当前步数值desc存入stepnum        
            }, 
            jumpto:function( index ) {
                    for(let i = 0;i < 9; i++){
                        this.list[i].val=history[index+1][i] 
                    }
                    this.list=this.list.slice()
                    //close.log(this.list)
                    //this.$forceUpdate()
            }   
        }
}
</script>

<style  scoped>
.extra{
    height: 33px;
}
</style>
  

    