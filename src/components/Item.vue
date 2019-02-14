<template>
    <div>
    <li :class="{root:true,'root-folder':isFolder && open}">
        <div
            :class="{bold: isFolder,folder:isFolder,plus:!open && isFolder,minus:open && isFolder}"

            @click="toggle"
            @dblclick="changeType">
            <span class="item-text">{{ model.name }}</span>
            <span v-if="isFolder" class="folder-heading">ITEM WITH DEPENDENCIES</span>
            <span v-if="!isFolder" class="folder-heading">DOUBLE CLICK TO ADD DEPENDENT</span>
            <div @click.stop="editingShow = !editingShow" class="edit"><span></span></div>
            <div @click.stop="changeItem" class="checkbox-done"><span></span></div>
            
            <div v-show="editingShow">
              <input @click.stop="" v-model="editingText" type="text" class="editing-input" placeholder="Input new Text Here" >
              <button @click.stop = editItem>Change</button>
            </div>
        </div>
        
        
        <ul v-show="open" v-if="isFolder">

            <div class="item-container">
            <item
              class="item"
              v-for="(model, index) in model.children"
              :key="index"
              :model="model" 
              @list-updated="$emit('list-updated')">
            </item>

            </div>
            

            <!-- <button class="show-adding" @click="addingShow = !addingShow">{{addingMessage}}</button> -->
            
            <div class="adding-container" v-show="addingShow">
              <input @keyup.enter="addChild" type="text" v-model="todoText">
              <button @click="addChild" >add</button>
            </div>
        </ul>
        
    </li>
    </div>
</template>
<script>

export default {
    name:'item',

    props: {
    model: Object,
    postUrl:String
  },
  data: function () {
    return {
        open: true,
        todoText:'',
        addingShow:true,
        editingShow:false,
        editingText:''
    }
  },
  computed: {
    isFolder: function () {
      return this.model.children //&& this.model.children.length
    },
    addingMessage:function(){
       return this.addingShow ? 'Hide Adding' : 'Show Adding'
    }
  },

  
  methods: {
    toggle: function () {
      if (this.isFolder) {
        this.open = !this.open
      }
    },

    changeType: function () {
      if (!this.isFolder) {
        this.$set(this.model, 'children', []);
        this.addChild();
        this.open = true;
      }
      
    },
    addChild: function () {
        if(this.todoText.length > 0){
          this.model.children.push({
            id:this.randomIdGenerator(),
            name: this.todoText
          })

          this.todoText = '';
          this.$emit('list-updated');
        }
      
        
    },

    changeItem(id){

        function deleteItem(node){
          
          if(node.$parent.$parent.$parent ===  undefined){
            return;
          }
          
          let index = node.$parent.model.children.indexOf(node.model);
          node.$parent.model.children.splice(index,1);
        } 

        deleteItem(this);
        this.$emit('list-updated');
        //uncoment when backend will be ready
        //axios.post(this.postUrl,id);
    },

    editItem(){
      
      this.editingShow = !this.editingShow;
      this.model.name = this.editingText;
      this.editingText = '';
      this.$emit('list-updated');

      //uncoment when back ready

      //axios.post(postUrl,this.model.id);
    },


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

            return Number(id);
        },
  },

  mounted(){
    if(this.$parent.$parent.$parent ===  undefined){
        document.querySelector('.checkbox-done').remove();
        document.querySelector('.edit').remove();
        return;
    }
  }
  
}
</script>

<style>
body {
  font-family: Menlo, Consolas, monospace;
  color: #444;
}

.bold {
  font-weight: bold;
}
ul {
  padding-left: 1.5em;
  line-height: 1.5em;
  text-align: left;
}

.root{
    list-style: none;
    width: 100%;
    
    box-sizing: border-box;
}

.root div{
  font-size: 22px;
}

.add{
  margin-top: 10px;
  font-size: 30px;
}

.folder{
  
  margin: 15px 0;
  cursor: pointer;
  color:#000080;
}

.root-folder{
  border-left: 3px solid #000080;
}

.plus:before{
  content: '+';
}

.minus:before{
  content: '-';
}

.show-adding{
  margin: 20px 0 0;
}

.item{
  margin: 10px 0px;
  transition: all .3s;
  padding-left: 15px;
}

.deleting{
  color:red;
}

input[type=text]{
  
  border:none;
  border-bottom: 1px solid black;
  background: none;
  outline: none;
  height: 20px;
  box-sizing: border-box;
}

button{
  margin: 0 10px;
  border: none;
  background: none;
  width: 100px;
  
  outline: none;
  cursor: pointer;
  background: #6200EE;
  text-transform: uppercase;
  color: #fff;
  font-weight: bold;
  transition: .3s;
}

button:hover{
  background: #208E79;
}

.adding-container{
  display: flex;
  padding: 15px;
  width: 300px;
  justify-content: space-between;
}

.folder-heading{
  font-size: 14px;
  text-transform: uppercase;
  color: #000;
  font-weight: bold;
}

.done{
  text-decoration: line-through;
}

.checkbox-done{
  display: inline-block;
  position: relative;
  top: 5px;
  width: 20px;
  height: 20px;
  cursor: pointer;
  z-index: 5;
  /* background: red; */
  
}

.checkbox-done span{
  display: block;
  width: 100%;
  height: 0px;
  background: red;
  position: absolute;
  top:50%;
  left:50%;
  transform: translate(-50,-50%);
}



.checkbox-done span:before{
  content: '';
  width: 100%;
  height: 4px;
  background: inherit;
  position: absolute;
  left:50%;
  top:50%;
  transform: translate(-50%,-50%) rotate(45deg);
}

.checkbox-done span:after{
  content: '';
  width: 100%;
  height: 4px;
  background: inherit;
  position: absolute;
  left:50%;
  top:50%;
  transform: translate(-50%,-50%) rotate(-45deg);
}

.edit{
  width: 30px;
  height: 30px;
  display: inline-block;
  /* background: black; */
  cursor: pointer;
  position: relative;
  top: 11px;
}

.edit:before{
  content: '';
  display: inline-block;
  width: 25px;
  height: 25px;
  position: absolute;
  left:0;
  top:0;
  background: url(./../assets/edit.svg);
  background-size: cover;
}

.editing-input{
  margin-left: 10px;
}
</style>
