<template>
    <div class="loader-container">
      <div class="loader"></div>
    </div>
  </template>
  
  <script>
  export default {
    name: 'Loader'
  };
  </script>
  
  <style scoped>
  .loader-container {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100%;
  }
  
  .loader {
    border: 10px solid rgba(0, 0, 0, 0.1);
    width: 42px;
    height: 42px;
    border-radius: 50%;
    border-left-color: #6833FF;
    animation: spin 1s ease infinite;
  }
  
  @keyframes spin {
    0% {
      transform: rotate(0deg);
    }
    100% {
      transform: rotate(360deg);
    }
  }
  </style>
  