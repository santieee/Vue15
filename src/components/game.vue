<template>
  <div>
    <div class="container" :style="{height: x*55+'px', width: y*55+'px'}">
      <div v-for="(blockX, iX) in x" :key="iX">
        <div 
          v-for="(blockY, iY) in y" 
          :key="iY"
          :class="x*y == (iY+1)+(iX*y)?'block empty':'block'" 
          :id="`x${iX+1}-y${iY+1}`"
          :style="{top: iX*55+'px', left: iY*55+'px'}"
          @click="move"
        >   
         {{arr[x*y == iY+(iX*y)? '&nbsp;' : iY+(iX*y)]}}
        </div>
      </div>
    </div>
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
      showWin: false,
      counter: 0,
    }
  },
  methods:{
    move(){
      let target = event.target;
      // получаем координаты target
      let [x,y] = target.id.split('-').map(e => e.replace(/\D+/g,""));
      let aroundBlocks = [
        document.querySelector(`#x${--x}-y${y}`),
        document.querySelector(`#x${x+2}-y${y}`),
        document.querySelector(`#x${++x}-y${++y}`),
        document.querySelector(`#x${x}-y${y-2}`)
      ]
      for(let block of aroundBlocks){
        try{
          if(block.classList[1]){
            return this.swap(target, block);
          }
        }catch{}
      }
    },
    swap(target, empty){
      [empty.style.left, target.style.left, empty.style.top, target.style.top, target.id, empty.id] = [target.style.left, empty.style.left, target.style.top, empty.style.top, empty.id, target.id];
      this.checkWin();
      this.counter++;
    },
    shuffle(){
       this.arr = new Array(this.x * this.y - 1).fill().map((e,i) => i+1).sort(() => Math.random() - 0.5);
       this.counter = 0;
    },
    checkWin(){
      let count = 1;
      for(let i=0; i<this.x; i++){
        for(let k=0; k<this.y; k++){
          if(count < this.x*this.y && count !== +document.querySelector(`#x${i+1}-y${k+1}`).textContent){          
            return;       
          }
          count++;
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
</style>