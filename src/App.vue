<template>
  <div id="app">
    <div class="container">
      
        <Tree :model="treeData"/>

        <div class="item-wrap">
          <item class="item" :model="treeData" :postUrl="postUrl"  @list-updated="updateTree"></item>
        </div>
    </div>
  </div>
</template>

<script>

//Librarys
import axios from 'axios';

// Components
import List from './components/List'
import Tree from './components/Tree'
import item from './components/Item'



var data = {
  id:14234234324,
  name: 'TODO Tree',
  children: [
    
  ]
}

export default {
  components:{
    List,
    Tree,
    item
  },
  
  name: 'app',

  data () {
    return {
     treeData:data,
     todoText:'',
     getUrl:'https://jsonplaceholder.typicode.com/todos',
     postUrl:''
    }
  },

  created(){
    //uncomment when backend will be enabled and change getUrl

    // axios.get(this.getUrl)
    //   .then(response => response.data)
    //   .then(data => this.treeData = data)
    //   .catch(err => console.log(err));
  },

  methods:{
        updateTree(){
          this.$emit('update-tree')
        },
  }
}
</script>

<style lang="scss">
  body{
    margin: 0;
    padding: 0;
    background: #F2F2F2;
    font-family: Arial,sans-serif;
    position: relative;
  }

  .container{
    width: 100%;
    height: 100%;
    display: flex;
    justify-content: space-around;
    flex-wrap: wrap;

    
  }

  .wrapper{
    width: 50%;
  }

  .item-wrap{
    width: 50%;
    margin-top: 100px;
    justify-content: center;;
  }

</style>
