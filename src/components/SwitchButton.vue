<script setup lang="ts">

</script>

<template>
  <div class="switch-button">
    <input class="switch-button-checkbox" type="checkbox"></input>
    <label class="switch-button-label" for="">
      <span class="switch-button-label-span">☀️</span>
    </label>
  </div>
</template>

<style scoped lang="scss">
* {
  box-sizing: border-box;
  padding: 0;
  margin: 0;
}

body {
  background-color: #155FFF;
  display: flex;
  align-items: center;
  justify-content: center;
  height: 100vh;
  font-family: sans-serif;
}

.switch-button {
  background: rgba(255, 255, 255, 0.56);
  border-radius: 30px;
  overflow: hidden;
  width: 120px;
  text-align: center;
  font-size: 18px;
  letter-spacing: 1px;
  color: #155FFF;
  position: relative;
  padding-right: 60px;
  margin-bottom: 20px;
  position: relative;

  &:before {
    content: "🌙";
    position: absolute;
    top: 0;
    bottom: 0;
    right: 0;
    width: 60px;
    display: flex;
    align-items: center;
    justify-content: center;
    z-index: 3;
    pointer-events: none;
  }

  &-checkbox {
    cursor: pointer;
    position: absolute;
    top: 0;
    left: 0;
    bottom: 0;
    width: 100%;
    height: 100%;
    opacity: 0;
    z-index: 2;

    &:checked + .switch-button-label:before {
      transform: translateX(60px);
      transition: transform 300ms linear;
    }

    & + .switch-button-label {
      position: relative;
      padding: 5px 0;
      display: block;
      user-select: none;
      pointer-events: none;

      &:before {
        content: "";
        background: #fff;
        height: 100%;
        width: 100%;
        position: absolute;
        left: 0;
        top: 0;
        border-radius: 30px;
        transform: translateX(0);
        transition: transform 300ms;
      }

      .switch-button-label-span {
        position: relative;
      }
    }
  }
}
</style>