<template>
  <div id="app">
    <h1>Simon The Game</h1>
    <div>
      <Game />
      <OptionsGame />
    </div>
  </div>
</template>

<script>
import Game from "./components/Game";
import OptionsGame from "./components/OptionsGame";
export default {
  data() {
    return {
      gameStarted: false,
      round: 4,
      randomNumberArray: [],
    };
  },
  methods: {
    randomSquares() {
      this.randomNumberArray = [];
      for (let i = 0; i < this.round; i++) {
        this.randomNumberArray.push(
          Math.round(Math.random() * (this.round - 1) + 1)
        );
      }
      console.log(this.randomNumberArray);
    },
  },
  name: "App",
  components: {
    Game,
    OptionsGame,
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin: 0 auto;
  margin-top: 60px;
  max-width: 300px;
  display: flex;
  flex-direction: column;
  align-items: center;
}
</style>
