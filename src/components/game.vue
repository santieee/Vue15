<template>
  <div>
    <transition name="fade">
      <div class="container" :style="{height: x*55+'px', width: y*55+'px'}">
        <div v-for="(blockX, iX) in x" :key="iX">
          <div 
            v-for="(blockY, iY) in y" 
            :key="iY"
            class='block'
            :style="{top: iX*55+'px', left: iY*55+'px'}"
            @click="move(x*y == iY+(iX*y)? '&nbsp;' : iY+(iX*y))"
          >   
           {{arr[x*y == iY+(iX*y)? '&nbsp;' : iY+(iX*y)]}}
          </div>
        </div>
      </div>
    </transition>
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
        try{
          if(aroundBlocks[i].target == ' '){
            return this.swap(num, aroundBlocks[i].id);          
          }
        }catch{}
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
<style lang="scss">
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
  }
  .block:hover{
    background-color: rgb(96, 205, 235, .6);
    cursor: pointer;
  }
  .under{
    margin-top: 20px;
  }
  .slide-fade-enter-active {
    transition: all .3s ease;
  }
  .slide-fade-leave-active {
    transition: all .8s cubic-bezier(1.0, 0.5, 0.8, 1.0);
  }
  .slide-fade-enter, .slide-fade-leave-to
  /* .slide-fade-leave-active до версии 2.1.8 */ {
    transform: translateX(10px);
    opacity: 0;
  }
</style>