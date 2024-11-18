<template>
  <div class="menu">
    <div class="header">
      Меню
    </div>
    <div class="buttons">
      <router-link to="/game">
        <PixelButton class="main-button">
          ИГРАТЬ
        </PixelButton>
      </router-link>
      <router-link to="/leaderboard">
        <PixelButton class="main-button">
          ЛИДЕРБОРД
        </PixelButton>
      </router-link>
      <div v-if="!isAuth">
        <router-link to="/login">
          <PixelButton class="main-button">
            ВОЙТИ
          </PixelButton>
        </router-link>
      </div>
      <div v-if="!isAuth">
        <router-link to="/register">
          <PixelButton class="main-button">
            РЕГИСТРАЦИЯ
          </PixelButton>
        </router-link>
      </div>
      <div v-if="isAuth">
        <router-link to="/login">
          <PixelButton @click="handleExit" class="main-button">
            ВЫЙТИ
          </PixelButton>
        </router-link>
      </div>
      <PixelButton @click="handleExitGame" class="exit1">
          ВЫЙТИ ИЗ ИГРЫ
      </PixelButton>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import PixelButton from "../components/FancyButton.vue";

export default defineComponent({
  components: {
    PixelButton
  },
  data() {
    let isAuth: boolean = false;
    return {
      isAuth
    };
  },
  async mounted() {
    this.checkAuth();
  },
  methods: {
    async handleExit() {
      localStorage.removeItem('token');
    },
    handleExitGame() {
      window.close();
    },
    async checkAuth() {
      if (localStorage.getItem('token'))
        this.isAuth = true;
      }
    }
  });
</script>

<style lang="css" scoped>
.menu {
  display: flex;
  flex-direction: column;
}

.leftSide {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 40%;
}

.info {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  width: 60%;
}

.about {
  height: 45vh;
}

.game-route {
  height: 55vh;
  align-self: flex-start;
}

.buttons {
  display: flex;
  flex-direction: column;
}


.exit {
  position: absolute;
  bottom: 102px;
}

.exit1 {
  position: absolute;
  bottom: 40px;
}

.btns {
  width: 9em;
  font-size: 4cqmin;
  margin-bottom: 0.5em;
}

.header {
  font-size: 30px;
  text-align: center;
  padding: 20px;
  color: #fcfcfc;
  font-weight: 700;
}
</style>
