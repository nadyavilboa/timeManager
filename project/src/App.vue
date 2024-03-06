<template>
  <TimeManager msg="Welcome to Your Vue.js App"/>
</template>

<script>
import TimeManager from './components/TimeManager.vue'

export default {
  name: 'App',
  components: {
    TimeManager
  }
}
</script>

<style>
  body {
    margin: 0;
  }

  #app {
    font-family: 'Oswald', sans-serif;
    font-size: 16px;
    font-weight: 400;
    line-height: 100%;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
  }

  .visually-hidden {
    position: absolute;
    width: 1px;
    height: 1px;
    margin: -1px;
    border: 0;
    padding: 0;
    white-space: nowrap;
    clip-path: inset(100%);
    clip: rect(0 0 0 0);
    overflow: hidden;
  }
</style>
