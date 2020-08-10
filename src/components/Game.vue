<template>
  <div>
    <ul class="game">
      <li
        v-for="square in this.squares"
        :key="square.id"
        :class="square.classes"
        :style="{ transition: transitionDelay / 2 + 'ms' }"
        @click="squareHandler(square)"
      >{{ square.id }}</li>
    </ul>
    <button @click="opacityHandler()">Старт</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      squares: {
        red: {
          id: 1,
          classes: ["game__square", "game__square_red"],
        },
        blue: {
          id: 2,
          classes: ["game__square", "game__square_blue"],
        },
        yellow: {
          id: 3,
          classes: ["game__square", "game__square_yellow"],
        },
        green: {
          id: 4,
          classes: ["game__square", "game__square_green"],
        },
      },
      activeClass: "game__square_active",
      transitionDelay: 1000,
      setTimeoutDelay: 500,
      round: 1,
      randomNumberArray: [],
      historyClick: [],
      roundFinish: false,
      lost: false,
      blockClick: true,
    };
  },
  methods: {
    opacityEffects() {
      this.randomNumberArray.forEach((elem, index) => {
        const arr = Object.keys(this.squares);
        const square = elem - 1;
        if (this.squares[arr[square]].id === elem) {
          this.squareEffects(this.squares[arr[square]], index);
        }
      });
    },
    squareEffects(square, index = undefined) {
      this.blockClick = true;
      const delay = this.setTimeoutDelay + this.transitionDelay / 2;

      const setClass = setTimeout(() => {
        square.classes.push(this.activeClass);
        clearTimeout(setClass);
      }, this.setTimeoutDelay);

      const deleteClass = setTimeout(() => {
        square.classes.splice(square.classes.indexOf(this.activeClass), 1);
        console.log(index + 1 === this.randomNumberArray.length);
        if (index + 1 === this.randomNumberArray.length) {
          this.setTimeoutDelay = 500;
          this.blockClick = false;
        }
        clearTimeout(deleteClass);
      }, delay);
      this.setTimeoutDelay = this.setTimeoutDelay + this.transitionDelay;
    },
    opacityHandler() {
      this.blockClick = true;
      this.randomSquares();
      this.opacityEffects();
    },
    randomSquares() {
      const arr = Object.keys(this.squares);
      for (let i = this.randomNumberArray.length; i < this.round; i++) {
        this.randomNumberArray.push(
          Math.round(Math.random() * (arr.length - 1) + 1)
        );
      }
      console.log(this.randomNumberArray);
    },
    squareHandler(square) {
      if (this.blockClick === false) {
        this.squareEffects(square);
        this.historyClick.push(square.id);
        this.historyClick.forEach((elem, index) => {
          if (elem === this.randomNumberArray[index]) {
            if (this.historyClick.length === this.randomNumberArray.length) {
              this.roundFinish = true;
              this.round = ++this.round;
              this.historyClick = [];
              this.opacityHandler();
            }
          } else {
            this.lost = true;
            this.roundFinish = false;
          }
        });
        console.log(
          "финиш: ",
          this.roundFinish,
          "Проиграл: ",
          this.lost,
          "раунд",
          this.round
        );
      }
    },
  },
};
</script>

<style lang="scss" scoped>
.game {
  list-style-type: none;
  padding: 0;
  display: flex;
  flex-wrap: wrap;
  max-width: 200px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.7);

  &__square {
    width: 100px;
    height: 100px;
    box-sizing: border-box;

    &_red {
      background-color: #ff5643;
      opacity: 0.6;
      &:hover {
        border-top: 1px solid black;
        border-left: 1px solid black;
      }
    }
    &_blue {
      background-color: dodgerblue;
      opacity: 0.6;
      &:hover {
        border-top: 1px solid black;
        border-right: 1px solid black;
      }
    }
    &_yellow {
      background-color: #feef33;
      opacity: 0.6;
      &:hover {
        border-bottom: 1px solid black;
        border-left: 1px solid black;
      }
    }
    &_green {
      background-color: #bede15;
      opacity: 0.6;
      &:hover {
        border-bottom: 1px solid black;
        border-right: 1px solid black;
      }
    }
    &_active {
      opacity: 1;
    }
  }
}
</style>
