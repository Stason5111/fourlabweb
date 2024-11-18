<template>
  <div class="pixelify-sans">
    <router-view></router-view>
    <footer class="footer">
      beta v1.0.0
    </footer>
  </div>
</template>

<script setup lang="ts">

</script>
<script>
import mitt from 'mitt';

const eventBus = mitt();
export { eventBus };
</script>

<style lang="css">
@import url('https://fonts.googleapis.com/css2?family=Pixelify+Sans:wght@400..700&display=swap');

body {
  background-color: #303030;
    background-image: url('./assets/ground-pattern.png');
    background-repeat: repeat;
    background-size: 200px;
    font-family: 'Zilla Slab', serif;
    font-weight: 700;
    margin: 0;
    padding: 0;
}

#app {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  align-items: center;
  height: 100vh;
}

.pixelify-sans {
  font-family: 'Pixelify Sans', sans-serif;
  font-optical-sizing: auto;
  font-weight: 400; /* Adjust weight as needed */
  font-style: normal;
}

.footer {
  width: 100%;
  text-align: center;
  padding: 10px 0;
  color: #ffffff;
  position:fixed;
  bottom: 0;
  left: 0;
}
</style>
