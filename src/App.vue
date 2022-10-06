<template>
  <div id="app">
    <div class="card">
      <div class="header">
        <div v-if="game_start == false">
          <h1 class="header-title">Pedra, Papel ou Tesoura</h1>
        </div>
        <div v-else>
          <span v-if="winner == 'player'"
            ><h1 class="header-title">Você venceu!</h1></span
          >
          <span v-if="winner == 'computer'"
            ><h1 class="header-title">Você perdeu!</h1></span
          >
          <span v-if="winner == 'empate'"
            ><h1 class="header-title">Empate!</h1></span
          >
        </div>
      </div>
      <div class="content">
        <div v-if="game_start == false">
          <div class="game-area">
            <span class="material-icons gamepad"> sports_esports</span>
          </div>
          <div class="game-plays">Faça sua jogada!</div>
        </div>
        <div v-else>
          <div class="game-area">
            <i
              :class="{
                'fa-regular fa-hand-scissors': move_player == 'Tesoura',
                'fa-regular fa-hand': move_player == 'Papel',
                'fa-regular fa-hand-back-fist': move_player == 'Pedra',
              }"
            ></i>
            <span class="versus"> VS </span>
            <i
              :class="{
                'fa-regular fa-hand-scissors': move_computer == 'Tesoura',
                'fa-regular fa-hand': move_computer == 'Papel',
                'fa-regular fa-hand-back-fist': move_computer == 'Pedra',
              }"
            ></i>
          </div>
          <button class="btn-restart" @click="restart()">RECOMEÇAR</button>
        </div>
      </div>
      <div class="footer" :class="{ '-disabled': game_start == true }">
        <a class="game-play" @click="startGame('Tesoura')">
          <i class="fa-regular fa-hand-scissors"></i>
        </a>
        <span class="divider"></span>
        <a class="game-play" @click="startGame('Papel')">
          <i class="fa-regular fa-hand"></i>
        </a>
        <span class="divider"></span>
        <a class="game-play" @click="startGame('Pedra')">
          <i class="fa-regular fa-hand-back-fist"></i>
        </a>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",

  data() {
    return {
      title: "Pedra, Papel ou Tesoura",
      game_start: false,
      move_player: "",
      move_computer: "",
    };
  },

  methods: {
    startGame(move_choice) {
      if (this.game_start == false) {
        this.game_start = true;
        this.move_player = move_choice;
        this.computerMove();
      }
    },

    computerMove() {
      let move = Math.floor(Math.random() * 30);

      if (move > 0 && move <= 10) {
        this.move_computer = "Pedra";
      } else if (move > 10 && move <= 20) {
        this.move_computer = "Papel";
      } else {
        this.move_computer = "Tesoura";
      }
    },

    restart() {
      this.game_start = false;
    },
  },

  computed: {
    winner() {
      if (this.move_player == this.move_computer) {
        return "empate";
      } else if (
        (this.move_player == "Pedra" && this.move_computer == "Papel") ||
        (this.move_player == "Papel" && this.move_computer == "Tesoura") ||
        (this.move_player == "Tesoura" && this.move_computer == "Pedra")
      ) {
        return "computer";
      } else if (
        (this.move_player == "Pedra" && this.move_computer == "Tesoura") ||
        (this.move_player == "Papel" && this.move_computer == "Pedra") ||
        (this.move_player == "Tesoura" && this.move_computer == "Papel")
      ) {
        return "player";
      } else {
        return "";
      }
    },
  },
};
</script>

<style lang="scss">
@import "@/assets/styles/reset.css";
@import url("https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.2.0/css/all.min.css");
@import url("https://fonts.googleapis.com/css2?family=Inter:wght@900&display=swap");
@import url("https://fonts.googleapis.com/css2?family=Knewave&display=swap");

$primaryColor: #961ef4;
$gradient: linear-gradient(84.45deg, #961ef4 0%, #e35858 100%);

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

.card {
  width: 90%;
  height: 400px;
  display: flex;
  flex-direction: column;
  align-items: center;
  position: relative;
  box-sizing: border-box;
  background: #ffffff;
  background-clip: padding-box; /* !importanté */
  border: solid 2px transparent; /* !importanté */
  border-radius: 1em;
  &:before {
    content: "";
    position: absolute;
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;
    z-index: -1;
    margin: -2px; /* !importanté */
    border-radius: inherit; /* !importanté */
    background: $gradient;
  }
}

.header {
  width: 105%;
  height: 90px;
  position: relative;
  top: -5px;
  background: $gradient;
  box-shadow: 0px 7px 5px -5px rgba(0, 0, 0, 0.25);
  border-radius: 10px;
  display: flex;
  flex-direction: column;
  justify-content: center;
}

.header-title {
  font-family: "Inter", sans-serif;
  font-style: normal;
  font-weight: 900;
  color: #ffffff;
  font-size: 1.8em;
}

.content {
  height: 50%;
  display: flex;
  flex-direction: column;
  justify-content: center;
}

.footer {
  width: 85%;
  height: 85px;
  display: flex;
  flex-direction: row;
  justify-content: space-around;
  align-items: center;
  border-radius: 10px;
  background-color: #3a2a45;
  color: #ffffff;
  font-size: 3em;

  &.-disabled {
    opacity: 0.5;

    .game-play {
      cursor: default;
    }
  }

  .game-play {
    cursor: pointer;
  }
}

.divider {
  width: 2px;
  height: 60%;
  background-color: #ffffff;
}

.game-area {
  font-size: 5em;
  background: $gradient;
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}

.game-plays {
  font-family: "Inter", sans-serif;
  font-size: 1.5em;
  background: $gradient;
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}

.gamepad {
  font-size: 1.3em;
}

.versus {
  font-family: "Knewave", cursive;
}

.btn-restart {
  background-color: #00a78e;
  color: #ffffff;
  font-family: "Inter", sans-serif;
  border: 0;
  border-radius: 20px;
  padding: 10px 20px;
  margin-top: 30px;
  cursor: pointer;
}

@media (min-width: 768px) {
  .card {
    width: 500px;
    height: 467px;
  }

  .header {
    width: 512px;
    height: 100px;
  }

  .footer {
    width: 380px;
    height: 92px;
  }

  .header-title {
    font-size: 2.2em;
  }

  .game-area {
    font-size: 7em;
  }
}
</style>
