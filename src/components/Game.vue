<template>
  <div id="template">
    <div class="my-content">
      <h1>{{ score }}</h1>
      <canvas id="my-game" width="500px" height="220px"></canvas>
      <div id="my-btns">
        <md-button @click="start" class="md-raised" v-if="gameStatus === 0">
          Start Game
        </md-button>
        <md-button @click="btnClick" class="md-raised" v-else-if="gameStatus === 1">
        <span class="material-icons">
          circle
        </span>
        </md-button>
        <md-button @click="newGame" class="md-raised" v-else-if="gameStatus === 2">
          New Game
        </md-button>
      </div>
    </div>
    </div>
</template>

<script>
export default {
  name: "Game",
  props: {},
  data: () => ({
    pos: 0,
    logoPath: "../img/Logo-icon.png",
    play: false,
    dir: -1,
    step: 2,
    colors: ["#4d4d4d", "green"],
    barriers: [],
    bsteps: 1,
    score: 0,
    probability: 0.96,
    gameStatus: 0,
    newB: -1
  }),
  mounted() {
    let canvas = document.getElementById("my-game");
    let ctx = canvas.getContext("2d");
    ctx.fillStyle = "white";
    ctx.fillRect(0, 0, 500, 500);
    ctx.fillStyle = "#4d4d4d";
    ctx.fillRect(0, 10, 500, 4);
    ctx.fillRect(0, 200, 500, 4);
    ctx.fillStyle = "#0071bc";
    ctx.fillRect(10, 14 + this.pos, 20, 20);
  },
  methods: {
    btnClick() {
      this.dir *= -1;
    },
    start() {
      this.gameStatus = 1;
      this.myInterval = setInterval(this.game, 15);
    },
    game() {
      this.newB--;
      // get canvas
      let canvas = document.getElementById("my-game");
      let ctx = canvas.getContext("2d");

      // draw background
      ctx.fillStyle = "white";
      ctx.fillRect(0, 0, 500, 500);
      ctx.fillStyle = "#4d4d4d";
      ctx.fillRect(0, 10, 500, 4);
      ctx.fillRect(0, 200, 500, 4);

      // draw player
      if (this.pos < 0){
        this.pos = 1;
        this.dir = 1;
      }
      if (this.pos > 166){
        this.pos = 165;
        this.dir = -1;
      }
      ctx.fillStyle = "#0071bc";
      if (this.pos === 0 || this.pos === 166) {
        this.dir *= -1;
      }
      this.pos += this.step * this.dir;
      ctx.fillRect(10, 14 + this.pos, 20, 20);

      // new barriers
      if (this.newB < 0) {
        let rnd = Math.random();
        if (rnd > this.probability) {
          this.newB = 70;
          let length = Math.random() * (100 - 20) + 20;
          let c = Math.random();
          if (c < 0.7)
            c = 0;
          else
            c = 1;
          this.barriers.push([500, Math.random() * (166 - length), length, c]);
        }
      }

      // draw barriers
      for (let i = 0; i < this.barriers.length; i++) {
        ctx.fillStyle = this.colors[this.barriers[i][3]];
        ctx.fillRect(this.barriers[i][0], 14 + this.barriers[i][1], 15, this.barriers[i][2]);
        this.barriers[i][0] -= this.bsteps;
      }

      // check if hits
      for (let i = 0; i < this.barriers.length; i++) {
        if (((30 >= this.barriers[i][0]) && (30 <= this.barriers[i][0] + 15)) && ((this.pos >= this.barriers[i][1]) && (this.pos <= this.barriers[i][1] + this.barriers[i][2]))) {
          this.hit(i);
        }
        if (((30 >= this.barriers[i][0]) && (30 <= this.barriers[i][0] + 15)) && ((this.pos + 10 >= this.barriers[i][1]) && (this.pos + 10 <= this.barriers[i][1] + this.barriers[i][2]))) {
          this.hit(i);
        }
        if (((10 >= this.barriers[i][0]) && (10 <= this.barriers[i][0] + 15)) && ((this.pos >= this.barriers[i][1]) && (this.pos <= this.barriers[i][1] + this.barriers[i][2]))) {
          this.hit(i);
        }
        if (((10 >= this.barriers[i][0]) && (10 <= this.barriers[i][0] + 15)) && ((this.pos + 10 >= this.barriers[i][1]) && (this.pos + 10 <= this.barriers[i][1] + this.barriers[i][2]))) {
          this.hit(i);
        }
      }
    },
    hit(index) {
      if (this.barriers[index][3] === 1) {
        this.score++;
      } else {
        this.gameStatus = 2;
        clearInterval(this.myInterval);
      }
      this.barriers.splice(index, 1);
    },
    newGame(){
      this.pos= 0;
      this.play = false;
      this.dir= -1;
      this.barriers= [];
      this.score = 0;
      this.gameStatus= 0;
      let canvas = document.getElementById("my-game");
      let ctx = canvas.getContext("2d");
      ctx.fillStyle = "white";
      ctx.fillRect(0, 0, 500, 500);
      ctx.fillStyle = "#4d4d4d";
      ctx.fillRect(0, 10, 500, 4);
      ctx.fillRect(0, 200, 500, 4);
      ctx.fillStyle = "#0071bc";
      ctx.fillRect(10, 14 + this.pos, 20, 20);
    }
  }
}
</script>

<style scoped>
.my-content {
  width: 500px;
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
}

#template {
  display: flex;
  justify-content: center;
}
</style>