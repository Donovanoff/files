<template>
    <div>
        
    </div>
</template>
<script>
export default {
    methods:{
        randomIdGenerator(){
            let max = 9;
            let min = 0;
            let count = 10;
            let id = ''
            let rand;

            for(let i = 0;i < count;i++){
                rand = min - 0.5 + Math.random() * (max - min + 1)
                rand = Math.round(rand);
                id+=rand;
            }

            this.todoItems.forEach(item => {
                if(item.id === id){
                    id = '';
                    for(let i = 0;i < count;i++){
                        rand = min - 0.5 + Math.random() * (max - min + 1)
                        rand = Math.round(rand);
                        id+=rand;
                    }
                    return id;
                }
            })

            return id;
        },
    }
}
</script>
<style>
    
</style>