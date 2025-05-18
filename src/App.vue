<template>
   <div class="app container">
  
    <div class="sidebar">
      <div class="sidebar-h1">
        <h1>PROWEB</h1>
      </div> 
      <nav> 
        <RouterLink to="/" exact-active-class="active-link" class="sidebar-link">
          <img src="./assets/img/starts.svg" alt="">
          <span>Запуски</span>
        </RouterLink>
        <RouterLink to="/about" exact-active-class="active-link" class="sidebar-link">
          <img src="./assets/img/about.svg" alt="">
          <span>Обо мне</span>
        </RouterLink>
       </nav>
    </div>

    <div class="main-content">
      <main>
        <RouterView />
      </main>
    </div>
     
   </div>
 </template>
 
 <script setup>

 </script>

 