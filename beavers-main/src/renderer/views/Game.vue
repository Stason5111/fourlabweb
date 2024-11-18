<script>

import Villager from "./Villager.vue";
import { useStateMachine } from "../stores/state";
import Leaderboard from "./Leaderboard.vue"; // Импортируем новый компонент

export default {
    name: "Game",
    components: {
        Villager,
        Leaderboard,
    },
    data() {
        return {
            villagers: [],
            card1: null,
            card2: null,
            trys: 0,
            carts : 0,
            isWin: false,
            leaderboardData: [],
        };
    },
    setup() {
        const state = useStateMachine();
        return {
            state
        }
    },
    methods: {
      addToLeaderboard(trys) {
        const username = localStorage.getItem('username');
        const leaderboardEntry = {
          username,
          trys
        };

      // Извлекаем текущие данные из localStorage
        const existingLeaderboard = JSON.parse(localStorage.getItem('leaderboard')) || [];

      // Добавляем новый элемент
        existingLeaderboard.push(leaderboardEntry);

      // Сохраняем обновленные данные обратно в localStorage
        localStorage.setItem('leaderboard', JSON.stringify(existingLeaderboard));
      },
      calculateTries() {
        return this.trys;
      },
      checkWin() {
        if (this.carts === 8){
          this.isWin = true;
          const trys = this.calculateTries();
          this.addToLeaderboard(trys);
          this.updateLeaderboard();

        }
      },
      goToMenu() {
        this.$router.push('/');
      },
      generateVillagers() {
        this.carts = 0
          for (let i = 0; i < 4 * 4; i++) {
              // generate a new random number
              let randomNumber = Math.floor(Math.random() * (4 + 4));
              // if the random number is already two times in the array, generate a new one
              if (this.getOccurrence(this.villagers, randomNumber) > 1) {
                  i--;
              } else {
                  this.villagers.push(randomNumber);
              }
          }
      },
      getOccurrence(array, value) {
          return array.filter((v) => (v === value)).length;
      },
      cardFlip(id) {
          if (this.checkWin() || id.flipped) {
              this.state.animation = false;
              return
          }

          if (this.card1 === null) {
              this.card1 = id;
              this.card1.flipped = false;
          } else if (this.card2 === null) {
              this.card2 = id;
              this.card2.flipped = false;
          }

          if (this.card1 !== null && this.card2 !== null) {
              this.state.animation = true;
              setTimeout(() => {
                  this.evaluate();
              }, 300)
          } else {
              this.state.animation = false;
          }

      },

      evaluate() {
          this.trys++;

          let timeout = 10

          if (this.card1.id === this.card2.id) {
              this.card1.justGuessed()
              this.card2.justGuessed()
              this.carts++
              this.checkWin();

          } else {
              this.card1.justFlip()
              this.card2.justFlip()
              timeout = 1000
          }

          this.card1 = null;
          this.card2 = null;

          this.checkWin();

          setTimeout(() => {
              this.state.animation = false;
          }, timeout)

      },
      reset() {
          this.villagers = [];
          this.card1 = null;
          this.card2 = null;
          this.trys = 0;
          this.isWin = false;
          this.carts = 0;
          setTimeout(() => {
              this.generateVillagers();
          }, 50)
      },
      updateLeaderboard() {
        this.leaderboardData.push({
          name: this.playerName,
          trys: this.trys
        });
      },
  },
  created() {
      this.villagers = [];
      this.generateVillagers();
  },
}

</script>

<template>

<div class="scores">
    <div>Versuche: {{ trys }}</div>
    <div><button class="reset-button" @click="reset">Reset</button></div>
    <div><button class="reset-button" @click="goToMenu">Вернуться в меню</button></div>

</div>

<div class="memo-grid" id="memo-grid">
    <Villager v-for="villager in villagers" :key="villager" :id="villager" @flipped="cardFlip" />
</div>
<div v-if="isWin" class="win-modal">
  <h2>Поздравляем! Вы победили!</h2>
  <p>Количество попыток: {{ trys }}</p>
  <button @click="reset">Сыграть еще раз</button>
  <button @click="goToMenu">Вернуться в меню</button>
</div>

</template>

<style>

.memo-grid {
    display: grid;
    grid-template-columns: repeat(4, 130px);
    grid-template-rows: repeat(4, 150px);
    grid-gap: 20px;
    align-items: center;
    justify-items: center;
    padding: 20px;
    width: min-content;
    margin: 0 auto;
}

.scores {
    width: 550px;
    margin: 10px auto;
    display: flex;
    justify-content: space-between;
    font-size: 2rem;
    align-items: center;
}

.reset-button {
    background-color: #75d3ae;
    border: none;
    outline: none;
    padding: 10px 20px;
    font-family: 'Zilla Slab', serif;
    font-weight: 700;
    font-size: 1.2rem;
    box-sizing: border-box;
    border-radius: 7px;
    cursor: pointer;
    transition: background-color .2s ease, box-shadow .2s ease;
    box-shadow: 0 5px 10px -5px #303030;
}

.reset-button:hover {
    background-color: #83e4bd;
}

.reset-button:focus {
    box-shadow: 0 5px 10px -5px #303030, 0 0 0 3px #75d3ad75;
}

.win-modal {
    position: fixed;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    background: white;
    padding: 20px;
    box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.5);
    z-index: 10;
}
</style>