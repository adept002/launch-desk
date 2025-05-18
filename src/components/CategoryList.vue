<template>
   <div class="category">
     <h2>Категории</h2>
     <div class="category-list">
       <button
         v-for="categ in categories"
         :key="categ.name"
         @click="$emit('select', categ.name)"
         :style="activeCategory === categ.name
           ? { backgroundColor: categ.color, color: 'white' }
           : { backgroundColor: '#ececec' }"
         class="btn"
       >
         {{ categ.name }}
       </button>
     </div>
   </div>
 </template>
 
 <script setup>
 defineProps({
   categories: Array,
   activeCategory: String
 });
 
 defineEmits(['select']);
 </script>
 
 