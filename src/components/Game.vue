<template>
    <div class="GAME" >
    </div>
</template>

<script>
    export default {
        name : 'GAME',
        data(){
            return {
                win : '',
            }
        },
        methods: {
            calculateWinner:function(list){
                const lines = [
                        [0, 1, 2],
                        [3, 4, 5],
                        [6, 7, 8],
                        [0, 3, 6],
                        [1, 4, 7],
                        [2, 5, 8],
                        [0, 4, 8],
                        [2, 4, 6],
                ];
                for (let i = 0; i < lines.length; i++) {
                    const [a, b, c] = lines[i];
                    if (list[a].val && list[a].val === list[b].val && list[a].val === list[c].val) 
                    {
                        this.win = list[a].val
                        return list[a].val
                    }
                }
            },
        }
}
</script>

<style scoped>

</style>