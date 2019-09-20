<template>
  <div id="app">
    <game v-if="game" @gameoff="game = false" :x="+x" :y="+y"></game>
    <div class="selectors" v-else>
      <label>Введите количество блоков в длину: 
        
      </label>
      <input type="number" min="2" max="10" name="x" v-model="x">
      <label>Введите количество блоков в ширину: 
        
      </label>
      <input type="number" min="2" max="10" name="y" v-model="y">
      <button @click="goGame">Go</button>
    </div>
    
  </div>
</template>

<script>
import game from './components/game.vue'
export default {
  name: 'app',
  components: {
    game
  },
  data(){
    return{
      game: false,
      x: 4,
      y: 4,
    }
  },
  methods:{
    goGame(){
      if(this.x < 11 && this.x > 1 && this.y < 11 && this.y > 1){
        this.game = true;
      }else{
        alert('Значение полей ограничено от 2 до 10!');
      }
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
}
.selectors{
  display: flex;
  flex-direction: column;
}
.selectors>input{
  text-align: center;
}
</style>
