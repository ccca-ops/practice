<template>
    <div> 
        <button class="square" @click="onclick(index)"  :disabled="dis">
        {{val}}
        </button>
    </div>
</template>

<script>
    export default {
        name : 'square',
        data(){
            return{
            dis : false,
            }
        },
        props: {
            index: {
                type: Number,
                default: null
            },
            val:{
                type:String,
                default:' '
            }
        },
        methods:{
           onclick:function(index){
                this.$emit('change',index);
                  
           }
        }
    }
</script>

<style  scoped>
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

</style>