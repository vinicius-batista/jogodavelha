<template>
  <div :class="{ app: true, 'bg-red': myTurn, 'bg-blue': !myTurn }">
    <div class="about">
      <p>Trabalho da disciplina de Computação Gráfica 2019-2</p>
      <p>Alunos: Vinicius, João Moniz e Euller</p>
    </div>

    <div class="grid" v-show="true">
      <div v-for="(block, index) in grid" @click="select(index)" :key="index">
        <block :figure.sync="block.figure"/>
      </div>
    </div>

    <transition v-on:enter="enterWin" v-bind:css="false">
      <win v-show="winner" @new-game="restart"></win>
    </transition>

    <transition v-on:enter="enterWin" v-bind:css="false">
      <draw v-show="!winner && draw" @new-game="restart"></draw>
    </transition>
  </div>
</template>

<script>
import block from "./components/block";
import win from "./components/win";
import draw from "./components/draw";

export default {
  name: "app",
  data() {
    return {
      grid: [
        { index: 0, figure: -1 },
        { index: 1, figure: -1 },
        { index: 2, figure: -1 },
        { index: 3, figure: -1 },
        { index: 4, figure: -1 },
        { index: 5, figure: -1 },
        { index: 6, figure: -1 },
        { index: 7, figure: -1 },
        { index: 8, figure: -1 }
      ],
      myTurn: false
    };
  },

  components: {
    block,
    win,
    draw
  },

  computed: {
    winner() {
      const wins = ["012", "036", "345", "147", "258", "678", "048", "246"];
      const player = this.myTurn ? 0 : 1;
      let moves = "";

      for (let i = 0; i < this.grid.length; i++) {
        if (this.grid[i].figure === player) {
          moves += i.toString();
        }
      }

      for (const win of wins) {
        if (
          moves.includes(win.charAt(0)) &&
          moves.includes(win.charAt(1)) &&
          moves.includes(win.charAt(2))
        ) {
          return true;
        }
      }

      return false;
    },
    draw() {
      const gridFull = this.grid.every(grid => grid.figure !== -1);
      return !this.winner && gridFull;
    }
  },

  methods: {
    select(index) {
      const { figure } = this.grid[index];

      if (figure > -1) {
        return;
      }

      this.grid[index].figure = this.myTurn ? 1 : 0;
      this.myTurn = !this.myTurn;
    },

    restart() {
      this.grid = [
        { index: 0, figure: -1 },
        { index: 1, figure: -1 },
        { index: 2, figure: -1 },
        { index: 3, figure: -1 },
        { index: 4, figure: -1 },
        { index: 5, figure: -1 },
        { index: 6, figure: -1 },
        { index: 7, figure: -1 },
        { index: 8, figure: -1 }
      ];
      this.myTurn = 0;
    },

    enter(el) {
      window.TweenMax.from(el, 1, {
        autoAlpha: 0,
        scale: 0,
        ease: window.Elastic.easeOut.config(1.25, 0.5)
      });
    },

    enterWin(el) {
      window.TweenMax.from(el, 1, {
        autoAlpha: 0,
        scale: 0,
        ease: window.Elastic.easeOut.config(1.25, 0.5)
      });
    }
  }
};
</script>

<style lang="scss">
@import url("https://fonts.googleapis.com/css?family=Permanent+Marker");

* {
  box-sizing: border-box;
}

body {
  margin: 0;
}

.about {
  color: white;
  background: #000;
  max-height: fit-content;
  height: fit-content;
  padding: 1em;
}

.app {
  display: flex;
  overflow: hidden;
  position: relative;
  width: 100vw;
  height: 100vh;
}

.bg-red {
  background-image: linear-gradient(red, orange);
}

.bg-blue {
  background-image: linear-gradient(blue, purple);
}

.grid {
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
  display: flex;
  flex-wrap: wrap;
  width: 300px;
  height: 300px;

  > div {
    flex: 0 0 auto;
    width: 100px;
    height: 100px;

    .block {
      cursor: pointer;
      display: table-cell;
      width: 100px;
      height: 100px;
      font: bold 50px/0 "Comic Sans MS", sans-serif;
      vertical-align: middle;
      text-align: center;
    }

    &:nth-child(1),
    &:nth-child(2),
    &:nth-child(3),
    &:nth-child(4),
    &:nth-child(5),
    &:nth-child(6) {
      border-bottom: 3px solid #fff;
    }

    &:nth-child(2),
    &:nth-child(5),
    &:nth-child(8) {
      border-left: 3px solid #fff;
      border-right: 3px solid #fff;
    }
  }

  span {
    display: block;
  }
}

.win {
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%) rotate(-10deg);
  transform-origin: 50% 50%;
  transform-origin: center;
  width: 300px;
  height: 300px;
  margin-top: -60px;
  margin-left: -20px;
  padding-top: 140px;
  font-family: "Permanent Marker", cursive;
  text-align: center;

  h2 {
    display: block;
    margin: 0 0 10px;
    font-size: 100px;
    text-shadow: 0px 0px 50px white;
    text-align: center;
  }

  button {
    cursor: pointer;
    transition: all 200ms cubic-bezier(0.175, 0.885, 0.32, 1.275);
    outline: none;
    display: inline-block;
    padding: 15px;
    background-color: darkred;
    border: none;
    border-radius: 10px;
    font-family: "Permanent Marker", cursive;
    font-size: 20px;
    color: white;

    &:hover {
      transform: scale(1.1);
    }
  }
}
</style>
