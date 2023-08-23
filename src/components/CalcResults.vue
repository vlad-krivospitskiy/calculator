<template>
    <div class="wrapper">
      {{ result }}
    </div>
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
     width: 375px;
     height: 45px;
     background: linear-gradient(223deg, #17181A 0%, #17181A 100%);
     color: #fff;
     font-family: 'Poppins';
     font-size: 48px;
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
   