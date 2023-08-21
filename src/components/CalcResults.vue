<template>
  <div class="wrapper">{{ result }}</div>
</template>

<script>
export default {
  name: "CalcResults",
  props: {
    result: String,
  },
};
</script>

<style scoped>
.wrapper {
  width: 175px;
  height: 72px;
  margin-right: 10px;
  background: linear-gradient(223deg, #17181a 0%, #17181a 100%);
  color: #818181;
  font-family: 'Poppins';
  font-size: 24px;
  font-style: normal;
  font-weight: 400;
  line-height: normal;
}
</style>
