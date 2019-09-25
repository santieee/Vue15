<template>
  <div>
    <transition-group 
      name="block" 
      tag="div" 
      class="container" 
      :style="{height: x*55+'px', width: y*55+'px'}"
    >    
      <div 
        v-for="(cell,i) in (x*y)"
        :key="arr[i]"
        :style="{top: Math.floor(i/y)*55+'px', left:i%y*55+'px'}"
        @click="move(x*y == i? '&nbsp;' : i)"
        class="block" 
      >
        {{arr[i]}}
      </div>
    </transition-group>
    <div v-text="counter" class="under"></div>
    <button @click="shuffle" class="under">Refresh</button>
    <br><hr>
    <button @click="$emit('gameoff')">Game off</button>
  </div>
</template>

<script>
export default {
  props:{
    x: {
      type: Number,
      validator: function (v){ return v < 11 && v > 1 }
    },
    y: {
      type: Number,
      validator: function (v){ return v < 11 && v > 1 }
    } 
  },
  data(){
    return{
      arr:[],
      counter: 0,
    }
  },
  methods:{
    move(num){
      let aroundBlocks = [
        {target: this.arr[num%this.y == 0 ? null :num-1], id: num-1},
        {target: this.arr[(num+1)%this.y == 0 ? null: num+1], id: num+1},
        {target: this.arr[num+this.y], id: num+this.y},
        {target: this.arr[num-this.y], id: num-this.y},
      ]
      for(let i in aroundBlocks){
        if(aroundBlocks[i].target == ' '){
          return this.swap(num, aroundBlocks[i].id);          
        }
      }
    },
    swap(target, empty){      
      [this.arr[target], this.arr[empty]] = [this.arr[empty], this.arr[target]];
      this.checkWin();
      this.counter++;
    },
    shuffle(){
       this.arr = new Array(this.x * this.y - 1).fill().map((e,i) => i+1).sort(() => Math.random() - 0.5);
       this.arr.push(' ');
       this.counter = 0;
    },
    checkWin(){
      for(let i = 0; i<this.arr.length; i++){
        if(this.arr[i] !== i+1){
          return
        } 
      }
      setTimeout(()=> alert('win!!!'), 220);
    }
  },
  mounted(){
    this.shuffle();
  }
}
</script>
<style>
  .container{
    position: relative;
    border: 1px solid black;
  }
  .block{
    position: absolute;
    display: inline-block;
    width: 50px;
    height: 50px;
    outline: 1px solid black;
    margin: 2px;
    font-size: 2.5em;
    transition: .2s;
    transition: all 0.5s;
  }
  .block:hover{
    background-color: rgb(96, 205, 235, .6);
    cursor: pointer;
  }
  .under{
    margin-top: 20px;
  }
  .block-enter, .block-leave-to, .block-leave-active  {
    opacity: 0;
    transform: scale(0);
  }
  .block-enter-to {
    opacity: 1;
    transform: scale(1);
  }
  .block-leave-active {
    position: absolute;
  }
  .block-move {
    opacity: 1;
    transition: all 0.5s;
  }
</style>