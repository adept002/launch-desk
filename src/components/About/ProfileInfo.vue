<template>
   <div class="profile-info">
    <div class="about-me">
      <h1>Обо мне</h1>
    </div> 

    <div class="info-group">
      <img :src="profilePhoto" alt="Фото"/>
      <div class="info">
        <h2>{{ fullName }}</h2>
        <p>{{ age }} лет | 18 октябрь 2003 год</p>
        <p>{{ position }}</p>
      </div>
    </div>

   </div>
 </template>
 
 <script setup>
 const profilePhoto = '/images/avatar.avif' 
 const fullName = 'Жумадуллаев Эльмурат Надирбекович'
 const birthDate = '2003-10-18'
 const position = 'Ташкентская область, Юкоричирчикский р-н.'
 
 function calcAge(dateStr) {
   const today = new Date()
   const birth = new Date(dateStr)
   let age = today.getFullYear() - birth.getFullYear()
   const m = today.getMonth() - birth.getMonth()
   if (m < 0 || (m === 0 && today.getDate() < birth.getDate())) {
     age--
   }
   return age
 }
 
 const age = calcAge(birthDate)
 </script>
 