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
      
      <h2>Blogs</h2>
      <!--code for blogs-->
    <ul>
      <li v-for="blog in blogs" :key="blog.title">
        <h3>{{ blog.title }}</h3>
        <p>{{ blog.content }}</p>
      </li>
    </ul>
  </div>
</template>

// add script and style tags

<script setup>

const name = 'Projetcs';
/*const projects = [
   {
    name : 'Project 1',
    description : 'This is project 1'
   },

   {
    name : 'Project 2',
    description : 'This is project 2'
   },
  ];*/
  const {data: projects, pending, error} = useFetch('http://localhost:5000/projects');
  
  //code for blogs
  const {data: blogs, pending2, error2} = useFetch('http://localhost:5000/blogs');
</script>

<style></style>

