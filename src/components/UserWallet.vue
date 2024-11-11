<template>
    <div>
      <h3>Wallet Balance: ${{ balance }}</h3>
    </div>
  </template>
  
  <script>
  export default {
    props: {
      balance: {
        type: Number,
        required: true
      }
    }
  };
  </script>
  
  <style scoped>
  h3 {
    color: green;
    font-weight: bold;
    font-size: xx-large;
    
  }
  </style>
  