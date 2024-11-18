<template>
    <div class="button-container">
      <button class="btn">
        <slot></slot>
      </button>
    </div>
  </template>

<script>
    
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Comfortaa:wght@300;400;500&family=Roboto:wght@700&display=swap');

.button-container {
  display: flex;
  justify-content: center;
  align-items: center; /* Full viewport height to center the button vertically */
}

.btn {
  font-size: 20px;
  color: #fcfcfc;
  margin-top: 10px;
  border-radius: 10px;
  padding: 10px 25px;
  font-family: 'Pixelify Sans', sans-serif;
  font-weight: 700;
  background: none;
  border: none;
  cursor: pointer;
}
</style>