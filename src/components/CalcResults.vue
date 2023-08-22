<template>
 <div class="wrapper" :class="{ 'dark-mode': isDarkMode }">{{ result }}</div>
</template>

<script>
export default {
  name: "CalcResults",
  props: {
    result: String,
    isDarkMode: Boolean,
  },
};
</script>

<style scoped>
.wrapper {
  width: 275px;
  height: 36px;
  background: linear-gradient(223deg, #17181a 0%, #17181a 100%);
  color: #fff;
  font-family: 'Poppins';
  font-size: 40px;
  font-style: normal;
  font-weight: 500;
  line-height: normal;
    overflow: hidden;
}
.dark-mode .wrapper {
  background: #f1f2f5;
backdrop-filter: blur(51px);
  color: #424242; 
}
</style>
