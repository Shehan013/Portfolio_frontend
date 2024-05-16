<template>
  <div>
    <h1>Shehan Lokuwella</h1>
    <p>IT Undergraduate of University of Moratuwa</p>
  </div>
  <div>
    <h2>{{ name }}</h2>
    <ul>
      <li v-for="project in projects" :key="project.name">
        <h3>{{ project.name }}</h3>
        <p>{{ project.description }}</p>
      </li>
    </ul>
  </div>
</template>

// add script and style tags

<script setup>

const name = 'Shehan Lokuwella';
const projects = [
   {
    name : 'Project 1',
    description : 'This is project 1'
   },

   {
    name : 'Project 2',
    description : 'This is project 2'
   },
  ];
</script>

<style></style>

