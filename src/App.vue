<template>
  <div id="app">
    <h1>Simon The Game</h1>
    <div class="main">
      <Game :speed="speed" />
      <OptionsGame :speed="speed" @change="setSpeed" />
    </div>
  </div>
</template>

<script>
import Game from "./components/Game";
import OptionsGame from "./components/OptionsGame";
export default {
  data() {
    return {
      speed: 1000,
    };
  },
  methods: {
    setSpeed(value) {
      this.speed = value;
    },
  },
  name: "App",
  components: {
    Game,
    OptionsGame,
  },
};
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin: 0 auto;
  margin-top: 60px;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.main {
  display: flex;
  max-width: 440px;
  justify-content: space-between;
  @media screen and (max-width: 440px) {
    flex-direction: column;
  }
}
</style>
