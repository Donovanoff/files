<template>
    <div class="wrapper">
        <transition name="fade">
            <div class="alert" v-show="alertShow">
                You dont Enter Any Text
            </div>
        </transition>
        <form>
            <div class="controls">
                <input v-model="itemText" type="text" class="text-input">
                <button @click="addItem" class="submit-input">ADD</button>
            </div>
        </form>
        <div class="items">
            <ul>
                <li v-for="item in todoItems" :key="item.id">
                   <span class="text">{{item.text}}</span>
                   <span @click="deleteItem(item)" class="delete">x</span>
                   <span @click="addDependentItem(item)" class="add-dependent-item">+</span>
                </li>
            </ul>
        </div>
    </div>
</template>
<script>
import axios from 'axios'

export default {
    name:'List',
    data(){
        return{
            itemText:'',
            todoItems:[],
            alertShow:false
        }
    },
    methods:{
        
        addItem(e){
            e.preventDefault();

            if(this.itemText.length > 0){
                this.todoItems.push({
                    'id':this.randomIdGenerator(),
                    'text':this.itemText,
                    'parent':null,
                    'childrens':[]
                })

                this.itemText = '';

                this.$emit('update')
            }else{
                if(!this.alertShow){
                    this.alertShow = true;
                    setTimeout(() => {
                        this.alertShow = false;
                    },2000)
                }
            }

            
 
        },

        addDependentItem(item){
            let newItem = {
                    'id':this.randomIdGenerator(),
                    'text':this.itemText,
                    'parent':item,
                    'childrens':[]
                }
            

           item.childrens.push(newItem);

           this.$emit('tree-update')
        },

        deleteItem(item){
            this.todoItems.forEach((elem,index) => {
                if(elem.id == item.id){
                    elem.childrens = [];
                    this.todoItems.splice(index,1);
                    return;
                }
            })
            
            this.$emit('tree-update');
        }
    },
    mounted(){
        
    }
    
}
</script>
<style lang="scss">

    

    form{
        width: 100%;
        margin: 0px auto;

        .controls{
            width: 50%;
            display: flex;
            margin: 30px auto 0;
            justify-content: center;
        }

        .text-input{
            margin: 5px 5px;
            display: block;
            border: none;
            background: transparent;
            border-bottom: 1px solid rgb(98,0,238);
            width: 70%;
            outline: none;
        }

        .submit-input{
            
            margin: 5px 5px;
            height: 30px;
            width: 30%;
            background:rgb(98,0,238);
            color: #fff;
            font-weight: bold;
            border: none;
            border-radius: 7px;
            font-size: 18px;
            cursor: pointer;
            outline: none;
        }
    }

    .items{
        width: 100%;
        text-align: center;

        ul{
            width: 100%;
            padding: 0;

            li{
                position: relative;
                width: 50%;
                margin: 10px auto;
                padding: 10px 0;
                list-style: none;
                min-height: 50px;

                .text{
                    text-align: left;
                    display: block;
                    font-size: 20px;
                }

                .delete{
                    display: block;
                    position: absolute;
                    width: 20px;
                    height: 20px;
                    top: 0px;
                    right: 10px;
                    color: red;
                    cursor: pointer;
                    font-size: 25px;
                }

                .add-dependent-item{
                    color: green;
                    display: block;
                    position: absolute;
                    bottom: 10px;
                    right: 10px;
                    cursor: pointer;
                    width: 20px;
                    height: 20px;
                    font-size: 25px;
                }
            }
        }
    }
   
    .alert{
        width: 90%;
        padding: 10px;
        background: rgba(242,81,76,0.6);
        margin: 15px 20px;
        text-align: center;
        font-size: 20px;
        text-transform: uppercase;
    }

    .fade-enter-active, .fade-leave-active {
        transition: opacity .5s;
    }

    .fade-enter, .fade-leave-to{
        opacity: 0;
    }
</style>