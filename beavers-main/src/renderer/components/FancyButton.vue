<template>
  <div class="container">
    <div class="center">
      <button class="btn" @click="playClickSound">
        <svg width="180px" height="60px" viewBox="0 0 180 60" class="border">
          <polyline points="179,1 179,59 1,59 1,1 179,1" class="bg-line" />
          <polyline points="179,1 179,59 1,59 1,1 179,1" class="hl-line" />
        </svg>
        <span><slot></slot></span>
      </button>
    </div>
  </div>
</template>

<script>
// Импортируйте ваш аудиофайл
import clickSound from '../assets/ss.mp3';

export default {
  data() {
    return {
      clickAudio: new Audio(clickSound)
    };
  },
  methods: {
    playClickSound() {
      this.clickAudio.play();
      this.$emit('click'); // Для передачи события клика родителю
    }
  }
};
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Pixelify+Sans:wght@400..700&display=swap');

.container {
  width: 180px;
  height: 60px;
  display: flex;
  justify-content: center;
  align-items: center;
  margin-bottom: 2px;
}

.center {
  width: 180px;
  height: 60px;
  position: relative;
}

.btn {
  width: 180px;
  height: 60px;
  cursor: pointer;
  background: transparent;
  border: 1px solid #aa70da;
  outline: none;
  transition: 1s ease-in-out;
  position: relative;
}

svg {
  position: absolute;
  left: 0;
  top: 0;
  fill: none;
  stroke: #fff;
  stroke-dasharray: 150 480;
  stroke-dashoffset: 150;
  transition: 1s ease-in-out;
}

.btn span {
  color: white;
  font-size: 18px;
  font-family: 'Pixelify Sans', sans-serif;
  font-weight: 400;
}
</style>
