<template>
  <div class="simon-game">
    <ul class="game">
      <li
        v-for="square in this.squares"
        :key="square.id"
        :class="square.classes"
        :style="{ transition: transitionDelay / 2 + 'ms' }"
        @click="squareHandler(square)"
      ></li>
    </ul>
    <button @click="opacityHandler()" class="game__button">Старт</button>
    <p v-if="lost === false">Раунд {{ round }}</p>
    <p v-if="lost === true">
      Отлично! Вы дошли до {{ round }} раунда. Попробуйте еще раз!
    </p>
  </div>
</template>

<script>
export default {
  props: {
    speed: Number,
  },
  data() {
    return {
      squares: {
        red: {
          id: 1,
          classes: ["game__square", "game__square_red"],
          audio: new Audio(require("@/sounds/1.mp3")),
        },
        blue: {
          id: 2,
          classes: ["game__square", "game__square_blue"],
          audio: new Audio(require("@/sounds/2.mp3")),
        },
        yellow: {
          id: 3,
          classes: ["game__square", "game__square_yellow"],
          audio: new Audio(require("@/sounds/3.mp3")),
        },
        green: {
          id: 4,
          classes: ["game__square", "game__square_green"],
          audio: new Audio(require("@/sounds/4.mp3")),
        },
      },
      activeClass: "game__square_active",
      clickActiveClass: "game__square_click-active",
      setTimeoutDelay: 500,
      round: 0,
      randomSquares: [],
      historyClick: [],
      lost: false,
      blockClick: true,
      opacityHandlerDelay: false,
    };
  },
  computed: {
    transitionDelay() {
      return this.opacityHandlerDelay === true ? 0 : this.speed;
    },
  },
  methods: {
    opacityEffects() {
      this.randomSquares.forEach((elem, index) => {
        const arr = Object.keys(this.squares);
        const square = elem - 1;
        if (this.squares[arr[square]].id === elem) {
          this.squareEffects(this.squares[arr[square]], index);
        }
      });
    },
    setClickActiveClass() {
      Object.values(this.squares).forEach((square) => {
        if (!square.classes.includes(this.clickActiveClass)) {
          square.classes.push(this.clickActiveClass);
        }
      });
    },
    deleteClickActiveClass() {
      Object.values(this.squares).forEach((square) => {
        if (square.classes.includes(this.clickActiveClass)) {
          square.classes.splice(
            square.classes.indexOf(this.clickActiveClass),
            1
          );
        }
      });
    },
    squareEffects(square, index) {
      setTimeout(() => {
        this.opacityHandlerDelay = false;
        const delay = this.setTimeoutDelay + this.transitionDelay / 2;
        const setClass = setTimeout(() => {
          square.classes.push(this.activeClass);
          square.audio.currentTime = 0;
          square.audio.play();
          clearTimeout(setClass);
        }, this.setTimeoutDelay);

        const deleteClass = setTimeout(() => {
          square.classes.splice(square.classes.indexOf(this.activeClass), 1);
          this.setTimeoutDelay = 500;
          if (index + 1 === this.randomSquares.length) {
            this.blockClick = false;
            this.opacityHandlerDelay = true;
            this.setClickActiveClass();
          }
          clearTimeout(deleteClass);
        }, delay);
        this.setTimeoutDelay = this.setTimeoutDelay + this.transitionDelay;
      }, this.setTimeoutDelay);
    },
    opacityHandler() {
      this.blockClick = true;
      this.randomSquares = [];
      this.round = 1;
      this.lost = false;
      this.setClickActiveClass();
      this.setRandomSquares();
      this.opacityEffects();
    },
    setRandomSquares() {
      const arr = Object.keys(this.squares);
      for (let i = this.randomSquares.length; i < this.round; i++) {
        this.randomSquares.push(
          Math.round(Math.random() * (arr.length - 1) + 1)
        );
      }
    },
    squareHandler(square) {
      if (this.blockClick === false) {
        square.audio.currentTime = 0;
        square.audio.play();

        this.historyClick.push(square.id);
        this.historyClick.forEach((elem, index, arr) => {
          if (elem === this.randomSquares[index]) {
            if (
              this.historyClick.length === this.randomSquares.length &&
              index + 1 === arr.length
            ) {
              this.round = ++this.round;
              this.blockClick = true;
              this.historyClick = [];
              this.deleteClickActiveClass();
              this.setRandomSquares();
              this.opacityEffects();
            }
          } else {
            this.lost = true;
            this.blockClick = true;
            this.historyClick = [];
            this.deleteClickActiveClass();
          }
        });
      }
    },
  },
};
</script>

<style lang="scss" scoped>
.simon-game {
  max-width: 240px;
}
.game {
  list-style-type: none;
  padding: 0;
  display: flex;
  flex-wrap: wrap;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.7);

  &__button {
    cursor: pointer;
    margin-top: 8px;
    padding: 6px 10px;
    background-color: #7b84db;
    border: 1px solid gray;
    border-radius: 6px;
    color: #fff;
    outline: none;
    &:active {
      box-shadow: inset 0 0 2px rgba(0, 0, 0, 0.9);
    }
  }

  &__square {
    width: 120px;
    height: 120px;
    box-sizing: border-box;

    &_red {
      background-color: #ff5643;
      opacity: 0.6;
      @media screen and (min-width: 768px) {
        &:hover {
          border-top: 1px solid black;
          border-left: 1px solid black;
        }
      }
    }
    &_blue {
      background-color: dodgerblue;
      opacity: 0.6;
      @media screen and (min-width: 768px) {
        &:hover {
          border-top: 1px solid black;
          border-right: 1px solid black;
        }
      }
    }
    &_yellow {
      background-color: #ffd500;
      opacity: 0.6;
      @media screen and (min-width: 768px) {
        &:hover {
          border-bottom: 1px solid black;
          border-left: 1px solid black;
        }
      }
    }
    &_green {
      background-color: #74e022;
      opacity: 0.6;
      @media screen and (min-width: 768px) {
        &:hover {
          border-bottom: 1px solid black;
          border-right: 1px solid black;
        }
      }
    }
    &_active {
      opacity: 1;
    }
    &_click-active {
      cursor: pointer;
      &:active {
        opacity: 1;
      }
    }
  }
}
</style>
