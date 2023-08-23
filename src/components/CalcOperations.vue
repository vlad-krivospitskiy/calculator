<template>
  <div class="wrapper">
    {{ operations }}
  </div>
</template>

<script>
export default {
  name: "CalcOperations",
  props: {
    operations: String, 
  },
};
</script>

<style scoped>
.wrapper {
  width: 375px;
  height: 36px;
  background: linear-gradient(223deg, #17181a 0%, #17181a 100%);
  color: #818181;
  font-family: "Poppins";
  font-size: 24px;
  font-style: normal;
  font-weight: 400;
  line-height: normal;
  overflow: hidden;
}
.dark-mode .wrapper {
  background: #f1f2f5;
  backdrop-filter: blur(51px);
  color: #424242;
}
</style>
