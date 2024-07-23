<template>
    <nav>
      <ul>
        <li><a href="/">Inicio</a></li>        
      </ul>
      <img class="logoAyesa" src="/Ayesa.png" alt="logo">
    </nav>
  </template>
  
  <script>
  export default {
    name: 'AppNav'
  };
  </script>
  
  <style scoped>
  nav {
    background-color: rgb(59 130 246 / 0.5);
    width: 100%;
    display: flex;
    justify-content: space-between;
  
  }
  
  nav ul {
    list-style: none;
    display: flex;
    align-items: center;
    gap: 1rem;
  }
  
  nav ul li {
    display: inline;
  }
  
  nav ul li a {
    color: rgb(0, 0, 0);
    text-decoration: none;
  }
  
  .logoAyesa{
    width: auto;
    height: 4rem;
  }
  </style>
  