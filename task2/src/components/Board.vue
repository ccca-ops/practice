<template>
    <div class="boardrow"> 
            <h2>winner is {{win}}</h2>
            <h3>NEXT PLAYER IS {{nowvalue}}</h3>
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
                win : '' ,
                desc :0,   //当前步数
                stepnum:Array(1).fill('Go to game star'),  //生成按钮的数组
                history:[ ['','','','','','','','',''] ],
            }  
        },
        components: {
            MySquare, MyGame,
        },
        methods: {
            changeval:function(index){
                    this.xISnext =! this.xISnext;
                    this.judge = this.$refs.game.calculateWinner(this.list);
                    //获胜者
                    this.win=this.$refs.game.win;         
                    
                    //禁用点击棋盘的：
                    while(this.judge === 'X' || this.judge === 'O'){
                        return null
                    }
                    if(this.xISnext == false){
                        this.list[index].val='X';
                        this.nowvalue = 'O';   //显示next玩家
                    }else{
                        this.list[index].val='O';
                        this.nowvalue = 'X'; 
                    }
                    this.desc=this.desc+1; //当前步数
                    this.stepnum[this.stepnum.length]='Go to move#'+this.desc;   //把当前步数值desc存入stepnum    
                    this.stepnum=this.stepnum.slice(0,[this.desc+1]);    //根据步骤数更新按钮数组
                    //历史数组
                    this.history[this.desc+1]=new Array();
                    for(let j = 0;j < 9;j++ ){
                        this.history[this.desc+1][j]=this.list[j].val;
                    }
                              
            }, 
            jumpto:function( index ) {  //这里的index相当于desc，是history的步骤
                    for(let i = 0;i < 9; i++){
                        if(index == 0)this.list[i].val='';    //点击gamestar情况
                        else this.list[i].val=this.history[index+1][i];
                    }
                    this.desc=index   //改变当前步骤数
                    if(this.desc%2 == 1 )this.xISnext=false;
                    if(this.desc%2 == 0 )this.xISnext=true;
            }       
        }
}
</script>

<style  scoped>
.extra{
    height: 33px;
}
</style>
  

    