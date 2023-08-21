<template>
  <div id="app">
    <CalcBody />
  </div>
</template>

<script>
import CalcBody from './components/CalcBody.vue';

export default {
  name: 'App',
  components: {
    CalcBody
  }
}
</script>

<style>
* {
  box-sizing: border-box;
  margin: 0;
}
</style>
